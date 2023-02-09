1. ubuntu20.04
####

1. plus
====

NGINX Plus のDocker Imageです

サンプルコンフィグの応答内容
----

- ``/`` : ``plus`` のテキストを応答する
- ``/api`` : NGINX Plus APIが応答する
- ``/dashboard.html`` : NGINX Plus Dashboard HTMLファイルを応答する

Buildコマンド
----

.. code-block:: cmdin

  ./buildNGINXcontainer.sh -o ubuntu20 -i plus -t plus -C nginx-repo.crt -K nginx-repo.key

2. plus-lua-njs
====

NGINX Plus + ``Lua Module , NJS Module`` のDocker Imageです

サンプルコンフィグの応答内容
----

NGINX Plusのイメージの応答内容との差分は以下のとおりです

- ``/`` : ``plus-lua-njs`` のテキストを応答する
- ``/njs`` : LUAのプログラムにより、リクエストの情報をjson形式で応答します
- ``/lua`` : LUAのプログラムにより、リクエストの情報をHTML形式で応答します

Buildコマンド
----

.. code-block:: cmdin

  ./buildNGINXcontainer.sh -o ubuntu20 -i plus-lua-njs -t plus-lua-njs -C nginx-repo.crt -K nginx-repo.key


3. plus-napd
====

NGINX Plus + ``NGINX App Protect Dos`` のDocker Imageです

サンプルコンフィグの応答内容
----

応答内容は、NGINX Plusのイメージと同じです。NGINX App Protect Dosが有効になっているため、それらのサンプルコンフィグが設定されています

Buildコマンド
----

  ./buildNGINXcontainer.sh -o ubuntu20 -i plus-napd -t plus-napd -C nginx-repo.crt -K nginx-repo.key

4. plus-napw
====

NGINX Plus + ``NGINX App Protect WAF`` のDocker Imageです

サンプルコンフィグの応答内容
----

応答内容は、NGINX Plusのイメージと同じです。NGINX App Protect WAFが有効になっているため、それらのサンプルコンフィグが設定されています。
攻撃トラフィックを実行するとデフォルトのブロックページが表示されます

Buildコマンド
----

  ./buildNGINXcontainer.sh -o ubuntu20 -i plus-napw -t plus-napw -C nginx-repo.crt -K nginx-repo.key

5. plus-napw-napd
====

NGINX Plus + ``NGINX App Protect WAF/DoS`` のDocker Imageです

サンプルコンフィグの応答内容
----

Buildコマンド
----

  ./buildNGINXcontainer.sh -o ubuntu20 -i plus-napw-napd -t plus-napw-napd -C nginx-repo.crt -K nginx-repo.key

6. plus-napw-napd-lua-njs
====

NGINX Plus + ``NGINX App Protect WAF/DoS, LUA Module, NJS Module`` のDocker Imageです

サンプルコンフィグの応答内容
----

Buildコマンド
----

  ./buildNGINXcontainer.sh -o ubuntu20 -i plus-napw-napd-lua-njs -t plus-napw-napd-lua-njs -C nginx-repo.crt -K nginx-repo.key

7. agent-plus
====

8. agent-plus-lua-njs
====

9. agent-plus-napd
====

10. agent-plus-napw
====

11. agent-plus-napw-napd
====

12. agent-plus-napw-napd-lua-njs
====