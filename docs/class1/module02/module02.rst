NGINX Docker ContainerのBuild
####

本手順ではいくつかのDocker ContainerのBuild手順を紹介します。

0. 必要なファイルの取得
====

.. code-block:: cmdin

  cd ~/
  git clone https://github.com/BeF5/f5j-nginx-docker.git
  cd f5j-nginx-docker/
  cp ~/nginx-repo* .

1. コンテナBuild用シェルスクリプト
----

コンテナBuild用のシェルスクリプトは以下の通りとなります

.. code-block:: cmdin

  ./buildNGINXcontainer.sh -h
  NGINX Docker image builder
  
   This tool builds a NGINX Plus Docker image
  
   === Usage:
  
   ./buildNGINXcontainer.sh [options]
  
   === Options:
  
   -h                     - This help
   -t [target image]      - Docker image name to be created
   -o [base OS image]     - base OS image name
   -i [image type]        - NGINX image type name
   -n [NMS URL]   - NMS(NGINX Management Suite) URL (https://nms-fqdn)
   -C [file.crt]          - Certificate file to pull packages from the official NGINX repository
   -K [file.key]          - Key file to pull packages from the official NGINX repository
   -D [NAP DoS Version]           - NAP DoS Version ithat is installed to Container
   -p                     - Push Docker image to registry
  
  === Target OS / NGINX image:
  |--<<base OS Image>>
  |  |--<<NGINX image type>>
  
  |-- ubuntu20
  |  |-- agent-plus
  |  |-- agent-plus-lua-njs
  |  |-- agent-plus-napd
  |  |-- agent-plus-napw
  |  |-- agent-plus-napw-napd
  |  |-- agent-plus-napw-napd-lua-njs
  |  |-- plus
  |  |-- plus-lua-njs
  |  |-- plus-napd
  |  |-- plus-napw
  |  |-- plus-napw-napd
  |  |-- plus-napw-napd-lua-njs

- ``Options`` にオプションとして指定可能なパラメータが表示されています
- ``Target OS / NGINX image`` に現在このコマンドでBuild可能なBase OSと、NGINXの構成が一覧になって表示されます

2. 基本的なディレクトリ構成
----

各NGINX Imageのディレクトリに必要となるファイルを保存しています。
基本的なディレクトリ構成は以下の通りです

- 例 : ubuntu20 / plus

+--------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|Dockerfile    | Container Image Buildの各種実行内容が記述されています                                                                                                                   |
+--------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|entrypoint.sh | Container Imageの実行を開始する際に実行するShell Scriptです。Container Image Build時にこのファイルをコピーし、Container Imageに含んでいます                             |
+--------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|nginx.conf    | NGINXのベースとなる設定ファイルです。Container Image Build時にこのファイルをコピーし、Container Imageに含んでいます                                                     |
+--------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|conf.d        | NGINXの各種設定ファイルを含むディレクトリです。Image毎に設定ファイルが異なります。Container Image Build時にこのファイルをコピーし、Container Imageに含んでいます        |
+--------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

設定ファイルは主に以下のような構成を取ります

- nginx.conf で必要なモジュールを読み込み、 ``conf.d`` 配下の ``*.conf`` ファイルを読み込みます
- conf.d/*.conf で通信を待ち受ける設定をします。 ``TCP 80`` でHTTPを待ち受けます。

  - ``/`` は、同NGINXが待ち受ける別のポート ``TCP 81`` に転送し、シンプルなテキストの応答を受けます
  - ``/api`` は、 ``NGINX Plus API`` に接続、応答します 
  - ``/dashboard.html`` は、 ``NGINX Plus Dasbhaord`` に接続、応答します

2. Docker Buildコマンド
----

- Base OS Image ``Ubuntu20.04`` , ``NGINX Plus`` Docker Image

.. code-block:: cmdin

  ./buildNGINXcontainer.sh  -o ubuntu20 -i plus -t plus -C nginx-repo.crt -K nginx-repo.key

- Base OS Image ``Ubuntu20.04`` , ``NGINX Plus / NGINX App Protect WAF`` Docker Image を RegistryにPush

.. code-block:: cmdin

  ./buildNGINXcontainer.sh  -o ubuntu20 -i plus-napw -t registry.example.com/root/nms/plus-napw:5.0 -C nginx-repo.crt -K nginx-repo.key -p

- Base OS Image ``Ubuntu20.04`` , ``NGINX Plus + NGINX Agent`` Docker Image

.. code-block:: cmdin

  ./buildNGINXcontainer.sh  -o ubuntu20 -i plus -t plus -C nginx-repo.crt -K nginx-repo.key -n "https://10.1.1.5"

3. Docker 実行
----

- docker Compose

Docker Compose file

.. code-block:: bash
  :linenos:
  :caption: Docker Compose Fileサンプル
  :emphasize-lines: 3,5-6,8-11

  services:
      nginx-gw:
          image: agent-plus-napw:latest
          hostname: agent-plus-napw
          ports:
          - "80:80"
          environment:
           - NMS_HOST=10.1.1.5
           - NMS_GRPC_PORT=443
           - NMS_INSTANCEGROUP=napw-cluster
           - NMS_TAG=napw-proxy

- ``3行目`` : 実行するDocker Image
- ``5-6行目`` : HTTP(80)で待ち受けた内容を、ContainerのHTTP(80)へマッピング
- ``8-11行目`` : NGINX Agent ありのDocker Imageの場合以下パラメータを指定して実行

  - ``NMS_HOST`` : NMSが待ち受けるIPアドレス(10.1.1.5)
  - ``NMS_GRPC_PORT`` : NMSが待ち受けるPort番号(443)
  - ``NMS_INSTANCEGROUP (option)`` : インスタンス接続時にインスタンスグループに登録する場合のグループ名(napw-cluster)
  - ``NMS_TAG (option)`` : インスタンス接続時にタグを付与して登録する場合のタグ(napw-proxy)

Docker Compose コマンドの実行

.. code-block:: cmdin

  docker-compose -f docker-compose-nginx.yaml up -d


- docker run

Docker コマンドの実行

.. code-block:: cmdin

  docker run --name plus -p 80:80 -d plus
