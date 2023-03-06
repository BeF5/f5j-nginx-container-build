環境
#######

実施環境
====

-  事前にラボ環境へのInviteを行っておりますので、メールをご確認ください
-  利用するコマンド： git, sudo, curl, docker, docker-compose
-  NGINX Trialライセンスの取得、ラボ実施ユーザのHome Directoryへ配置

ラボ環境 (UDF(Unified Demonstration Framework)) コンポーネントへの接続
====

| 弊社が提供するLAB環境を使って動作を確認いただきます。
| ラボ環境を起動する等、一部ブラウザを使って操作します。
| Google ChromeがSupportブラウザとなります。その他ブラウザでは正しく動作しない場合があることご了承ください。
| 参照： `UDF Supported Browsers and Clients <https://help.udf.f5.com/en/articles/3470266-supported-browsers-and-clients>`__


Windows Jump HostへのRDP接続
----------------------------

Windows Jump HostからCLIの操作を行う場合、以下タブからRDP Clientファイルをダウンロードいただき接続ください

   .. image:: ./media/udf_jumpbox.png
      :width: 200

.. NOTE::
   | RDPのUser名、パスワードはDETAILSをクリックし、GeneralのタブのCredentialsの項目を参照ください
   | ``Administrator`` でログインしてください 

   - .. image:: ./media/udf_jumpbox_loginuser.png
       :width: 200
    
   - .. image:: ./media/udf_jumpbox_loginuser2.png
       :width: 200
   
Windows Jump Hostへログインいただくと、SSHClientのショートカットがありますので、そちらをダブルクリックし
マニュアルで示すホストへ接続ください

   - .. image:: ./media/putty_icon.jpg
      :width: 50

   - .. image:: ./media/putty_menu_kic.jpg
      :width: 200

Dockerについて
====

Dockerコンテナ とは
- 効率的な開発を行うために開発された仮想化技術
- アプリケーションを小さな単位でパッケージにして管理する
- 即座に起動、削除、どこでも同じアプリを利用できる


NGINXの各種構成に関するContainerイメージをBuildするためのDocker Fileのサンプル、それらを動作させる方法を確認いただけます
