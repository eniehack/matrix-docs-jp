# Debianの環境上でaptを用いてSynapseを構築する

以下の内容はQiitaにNakayaが投稿した「[MatrixのHomeserver、SynapseをDebian
10にインストールする](https://qiita.com/eniehack/items/268f5903d50fcd8dc309)」の記事をそのまま移植したものです。

------------------------------------------------------------------------

\<markdown> \# TL;DR

\* matrix-jp.netというサーバを立てたよ \*
レファレンス実装のサーバである、SynapseでMatrixサーバを構築していく記事だよ
\* Synapseはインストールが簡単だから是非インストールしてみてね \*
matrixの規格についてはまだわからない部分があるので追い追い記事にしていくよ

\# 経緯

私は前々からIMなどに興味があり、XMPPなどを好んで使ってたりしてきたのですが、まえから気になっていたmatrixというチャットプロトコルがあり、試してみたいと思って最近matrix-jp.net
というサーバを立てました。

\# Matrix とは

https://en.wikipedia.org/wiki/Matrix\_(protocol) をざっくりまとめると

\*
グループコミュニケーションからインスタントメッセンジャーはもちろん、VoIP、IoTまでを見据えたプロトコル
\* オープンかつRESTfulな規格 \* JSONを用いて通信 \* 通信は暗号化可能 \*
KDEが標準のコミュニケーション手段として採用し、インスタンスを運営？ \*
2019年6月にAPIの規格がv1.0になった \*
Bridge機能を用いることでIRCやSlackなど有名なコミュニケーション手段とも接続可能

詳しいMatrixの概要については先程述べたとおり、後でまとめようと思います。また\[公式サイト\]([https://matrix.org)にも詳細はあります](https://matrix.org)にも詳細はあります)。

\# インストール

## 注意

今回の記事ではSynapseのインストールのみに焦点を当てます。そのため、SSHやLet\'s
Encrypt、Webサーバなどの導入や設定に関しては他の記事を読んでください。
また、SynapseではAnsibleやDockerを用いての構築方法を\[紹介しています\]([https://github.com/matrix-org/synapse/blob/master/INSTALL.md#prebuilt-packages)が、今回は用いません](https://github.com/matrix-org/synapse/blob/master/INSTALL.md#prebuilt-packages)が、今回は用いません)。
この記事は私がインストールした方法を再構成したものです。再構成がうまくいっていない可能性があるので問題があったのであれば場合は連絡をください。基本的には\[Synapse公式のインストール方法\]([https://github.com/matrix-org/synapse/blob/master/INSTALL.md)を踏襲しているので、詳細はそちらに詳しいです](https://github.com/matrix-org/synapse/blob/master/INSTALL.md)を踏襲しているので、詳細はそちらに詳しいです)。
Debian以外のLinuxディストリビューションやBSDへのインストール方法もSynapse公式が\[紹介しています\]([https://github.com/matrix-org/synapse/blob/master/INSTALL.md)ので他ディストリビューションでもできます](https://github.com/matrix-org/synapse/blob/master/INSTALL.md)ので他ディストリビューションでもできます)。

## インストール手順

おおかたの順序はこのようになっています。

1\. Synapseのインストール 2. リバースプロキシ,PostgreSQLの設定 3.
Synapseの設定ファイルをいじる 4. Adminユーザの作成 4. Federationの設定

## 環境

始めに、今回用いたソフトウェアのバージョンを列挙しておきます。

\`\`\`shell-session \$ uname -vm #1 SMP Debian 4.19.37-5 (2019-06-19)
x86_64 \`\`\` \`\`\`shell-session \$ sudo nginx -v nginx version:
nginx/1.14.2 \`\`\` \`\`\`shell-session \$ sudo ufw version ufw 0.36
Copyright 2008-2015 Canonical Ltd. \`\`\` \`\`\`shell-session \$ sudo
psql \--version psql (PostgreSQL) 11.5 (Debian 11.5-1+deb10u1) \`\`\`
\`\`\`shell-session \$ apt list \| grep matrix-synapse-py3
matrix-synapse-py3/unknown,now 1.3.1+buster1 amd64 \[installed\] \`\`\`

## Synapseのインストール

SynapseはPython3製であり、\[PyPIにも登録されています\](<https://pypi.org/project/matrix-synapse/>)。そのためpipからでも導入は可能です。
その他にも御膳立てされた状態(prebuilt)で提供されているものもDebian公式リポジトリとSynapse公式aptリポジトリでパッケージが提供されています。しかし、Debian公式リポジトリmainのSynapseはバージョンが0.99とやや遅いので、ここではSynapse公式リポジトリからインストールすることにします。

まず、依存パッケージをインストール。

\`\`\`shell-session \$ sudo apt update \$ sudo apt upgrade \$ sudo
apt-get install build-essential python3-dev libffi-dev \\

                       python-pip python-setuptools sqlite3 \
                       libssl-dev python-virtualenv libjpeg-dev libxslt1-dev

\`\`\`

次にSynapse公式リポジトリをaptに登録。

\`\`\`shell-session sudo apt install -y lsb-release wget
apt-transport-https sudo wget -O
/usr/share/keyrings/matrix-org-archive-keyring.gpg
<https://packages.matrix.org/debian/matrix-org-archive-keyring.gpg> echo
\"deb \[signed-by=/usr/share/keyrings/matrix-org-archive-keyring.gpg\]
<https://packages.matrix.org/debian/> \$(lsb_release -cs) main\" \|

      sudo tee /etc/apt/sources.list.d/matrix-org.list

\`\`\`

Synapseをインストール。

\`\`\`shell_session \$ sudo apt update \$ sudo apt-get install
matrix-synapse-py3 \`\`\`

これでSynapseのインストールは完了です。

## リバースプロキシの設定

私はNginxを用いていますが、もちろんApacheなどでもリバースプロキシできます。

<https://ssl-config.mozilla.org/> と
\[Matrixの公式設定の例\]([https://github.com/matrix-org/synapse/blob/master/docs/reverse_proxy.rst)を組み合わせて適当な名前にして保存](https://github.com/matrix-org/synapse/blob/master/docs/reverse_proxy.rst)を組み合わせて適当な名前にして保存)。

\`sudo nginx
-t\`して問題がなければnginxリスタートしてnginxの設定は完了です。

nginx設定後に私はCertBotを動かして証明書を取得しました。

## PostgreSQLの設定 (optional)

Synapseのデフォルト設定ではSQLiteがデータベースとして設定されていますが、PostgreSQLも利用できます。ここではPostgreSQLのuserとdatabaseの作成を行っていこうと思います。Synapseにデータベースの場所を記述するのは次項で解説します。

まず、Synapseのデータを操作するsynapse_userというユーザを作ります。

\`\`\`shell-session su - postgres createuser \--pwprompt synapse_user
\`\`\`

次にPostgreSQLのコンソールを立ち上げ、synapse_userを所有者とする以下のデータベースを作成します。

\`\`\`shell-session psql=# CREATE DATABASE synapse ENCODING \'UTF8\'
LC_COLLATE=\'C\' LC_CTYPE=\'C\' template=template0 OWNER synapse_user;
\`\`\`

参考リンク:
<https://github.com/matrix-org/synapse/blob/master/docs/postgres.rst>

## Synapseの設定

##\# homeserver.yamlの編集

\`homeserver.yaml\`はSynapseの設定を記述されているファイルです。Synapseのaptリポジトリ経由でインストールした場合、\`/etc/matrix-synapse/homeserver.yaml\`にあります。

まずは

\`\`\`shell-session cat /dev/urandom \| tr -dc \'a-zA-Z0-9\' \| fold -w
32 \| head -n 1 \`\`\`

の結果を\`registration_shared_secret\`の値に入れます(ちなみにこの値がバレるとAdminアカウントが勝手に作られてしまうそうなので厳重に管理してください\[要出典\])。

次に\`enable_registration:
true\`に設定してください(でないと自身のアカウントも作れなくなってしまうみたいです\[要検証\])。

3つ目に\`homeserver.yaml\`の以下のような部分を探してきて、コメントアウトしてください。

\`\`\`yaml

1.  port: 8448

```{=html}
<!-- -->
```
      type: http
      tls: true
      resources:
        - names: [client, federation]

\`\`\`

#### PostgreSQLの設定

\`homeserver.yaml\`には\`database:\`というパラメータがあります。そこにデータベースのホスト、ポート、ユーザ名、パスワードを書いていきます。ちなみにパラメータは\`psycopg2.connect()\`に準拠しているらしいです。

参考リンク:
<https://github.com/matrix-org/synapse/blob/master/docs/postgres.rst>

##\# Let\'s Encryptの鍵を複製する

サーバ間での通信もTLSによって暗号化されるようためには鍵が必要なようなのですが、これには2つ方法があります。

1\. ACMEチャレンジをSynapseにさせる 2. Let\'s
Encryptから発行された鍵をそのまま使う

私はnginxの項で述べた通り、すでにLet\'s
Encryptから鍵を取得したので後者の方法を取りたいと思います。

ます取得した鍵をコピーしてきます。

\`\`\`shell-session \$ sudo cp
/etc/letsencrypt/live/{yourdomain}/fullchain.pem
/etc/matrix-synapse/fullchain.pem \$ sudo cp
/etc/letsencrypt/live/{yourdomain}/privkey.pem
/etc/matrix-synapse/privkey.pem \`\`\`

\`matrix-synapse-py3\`パッケージはmatrix-synapse.serviceを同梱しています。
また、matrix-synapse.serviceを実行するのはmatrix-synapseユーザです。なので、コピーしてきた鍵をグループに所有させてpermission
deniedさせるのを防ごうという感じです。

\* groupaddでmatrix グループを作成

\`\`\`shell-session \$ sudo groupadd matrix \`\`\`

\* matrix groupにmatrix-synapse userを追加

\`\`\`shell-session \$ sudo usermod -G matrix \`\`\`

\* chown
-Rで\`/etc/matrix-synapse\`をrootからmatrixグループに所有者を変更

\`\`\`shell-session \$ sudo chown -R root:matrix /etc/matrix-synapse
\`\`\`

\*
\`homeserver.yaml\`を開いて、\`tls_certificate_path\`に\`fullchain.pem\`のパスを、\`tls_private_key_path\`に\`privkey.pem\`のパスを書く

\`\`\`yaml tls_certificate_path: /etc/matrix-synapse/fullchain.pem
tls_private_key_path: /etc/matrix-synapse/privkey.pem \`\`\`

\* \`matrix-synapse.service\`をrestartする

\`\`\`shell-session \$ sudo systemctl restart matrix-synapse \`\`\`

## Adminユーザの作成

matrix-synapse.serviceが動作していることを確認して、以下のコマンドを実行してください。

\`\`\`shell_session \$ sudo register_new_matrix_user -u {youruserid} -p
{yourpassword} -c /etc/matrix-synapse/homeserver.yaml <http://localhost>
\`\`\`

## Federationの設定

他のサーバーとやりとりをするには

1\. DNSのSRVレコードを追加する 2.
\`/.well-known/matrix/server/\`にjsonを配置する

の2つの方法があります。ちなみに、先に\`/.well-known/matrix/server/\`を覗いてみて何もなかったらSRVを見にいくようになっているみたいです。

##\# SRVレコードを書く

以下の表のようにSRVレコードを登録します。

  ---------- -----------------------------
  項目       内容
  :-:        :-:
  Name       \_matrix.\_tcp.{yourdomain}
  Target     {yourdomain}
  Port       443
  Priority   10
  Weight     5
  ---------- -----------------------------

##\# Federationの確認

\`[https://matrix.org/federationtester/api/report?server_name={yourdomain}\`を確認して、エラーがなければ成功です](https://matrix.org/federationtester/api/report?server_name=%7Byourdomain%7D%60を確認して、エラーがなければ成功です)。

\# その他Synapseでできること

今回取り扱わなかったものが以下のように幾つかあります。これらも\[公式Docs\]([https://github.com/matrix-org/synapse/blob/master/docs/)を参照すれば導入できると思いますので是非やってみてください](https://github.com/matrix-org/synapse/blob/master/docs/)を参照すれば導入できると思いますので是非やってみてください)。

\* STUNを用いてVoIP \*
WebクライアントであるRiotをサーバにホスティングする \*
SMTPサーバと連携してパスワードリセットメールの送信

\# さいごに

matrixはサーバもクライアントもspecが公開されているので、それぞれいくつもの実装があります(もちろんSynapse以外にもサーバ実装は存在します)。また、クライアント向けSDKやBotフレームワークが豊富にあります。是非開発にしてみてもいいかもしれません。\[ここ\]([https://matrix.org/docs/projects/try-matrix-now)にクライアント、サーバ、Botフレームワークの一覧があります、いろいろ試してもいいでしょう](https://matrix.org/docs/projects/try-matrix-now)にクライアント、サーバ、Botフレームワークの一覧があります、いろいろ試してもいいでしょう)。

ところで、matrix-jp.netには\[#matrix-research:matrix-jp.net\]([https://matrix.to/#/#matrix-research:matrix-jp.net)や\[#negao_room:matrix-jp.net](https://matrix.to/#/#matrix-research:matrix-jp.net)や%5B#negao_room:matrix-jp.net)\]([https://matrix.to/#/#negao_room:matrix-jp.net)などの部屋があり、わちゃわちゃやっているので是非来てみてください](https://matrix.to/#/#negao_room:matrix-jp.net)などの部屋があり、わちゃわちゃやっているので是非来てみてください)。

といってもまだクライアントの説明がまだですけど......。それは近々どこかに書きたいと思います。

ではでは、よいMatrixライフを！

\# 参考文献

\* <https://github.com/matrix-org/synapse/blob/master/INSTALL.md> \*
<https://github.com/matrix-org/synapse/blob/master/docs/federate.md> \*
<https://github.com/matrix-org/synapse/blob/master/docs/postgres.rst>
\</markdown>
