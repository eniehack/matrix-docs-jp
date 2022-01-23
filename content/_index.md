# Welcome to matrix-docs-jp.netlify.app!

~~matrix-jp.netにようこそ！~~
matrix-jp.netは2021/9/25に閉鎖されました。
wiki.matrix-jp.netのドキュメントをGitHubリポジトリ、netlifyへそのまま移行した上で公開しています。

ここはmatrix.orgの規格やサーバ、クライアントなどについてまとめる日本語のWikiです。
[GitHubのリポジトリ](https://github.com/eniehack/matrix-docs-jp)にissueやPull Requestを投げることで誰でも編集に参加できます。

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="クリエイティブ・コモンズ・ライセンス" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />この サイト は <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">クリエイティブ・コモンズ 表示 4.0 国際 ライセンス</a>の下に提供されています。

## 初めての方へ

[FAQ]({{< relref "faq" >}})を先にお読みいただくことをお勧めします。

## for User

-   [Matrixとは？]({{< relref "users/what_is_matrix" >}})
-   Matrixを使ってみる
    -   [くりむさんによる「matrixの初め方」](https://crim0404.blogspot.com/2021/03/matrix_25.html)
    -   [tateisuさんによる記事「ガイド：ElementのPCデスクトップ版をインストールしてアカウントを作って部屋に入るまで」](https://lemmy.juggler.jp/post/786)
-   [主なMatrixインスタンス]({{< relref "users/instances" >}})
-   主な公開部屋リスト
    -   [tateisuさんによる日本matrix勢の公開部屋リスト](https://matrix-room-list-jp.netlify.app/)
-   主なMatrixクライアント
    -   [Web]({{< relref "users/web-client" >}})
    -   [PC]({{< relref "users/desktop-client" >}})
    -   [スマホ]({{< relref "users/mobile-client" >}})
-   [Bridgesとは？]({{< relref "users/bridges" >}})

## for Developer

-   Matrixの仕様
-   カスタムステッカー
    -   [やまこさんによる「Matrixにカスタムスタンプを設定する」](https://windish.kibe.la/shared/entries/18649f13-86b2-43af-8b41-89f6eb391121)
    -   [tateisuさんによる「カスタムステッカーパックを作って/使ってみる」](https://lemmy.juggler.jp/post/818)
    -   [tateisuさんによる「Matrix sticker changer」](https://matrix.juggler.jp/stickerChanger.html?name=%E3%81%AA%E3%81%8B%E3%82%88%E3%81%97%20%E3%81%8A%E3%81%AD%E3%81%88%E3%81%95%E3%82%93%E3%81%9F%E3%81%A1%20%E3%82%B9%E3%82%BF%E3%83%B3%E3%83%97&sender=%40yamako%3Amatrix.fedibird.com&url=https%3A%2F%2Fywindish.github.io%2Fstickerpicker%2Fweb%2F%3Ftheme%3D%24theme)

### Bot開発

-   Botの作り方
    -   Botフレームワーク

### クライアント開発

-   matrixクライアントの作り方
    -   SDK

### サーバ開発

-   Matrixサーバの作り方

## for Server Admin

### サーバ構築

-   Synapseの構築
    -   on Docker
        -   [tateisuさんによるSynapse on
            Docker構築メモ](https://lemmy.juggler.jp/post/759)
    -   [aptを用いたDebianでの構築]({{< relref "admins/install-synapse-on-debian-using-apt" >}})
    -   [URLプレビューを有効にする](https://lemmy.juggler.jp/post/794)
    -   [tateisuさんによる記事「ホームサーバのホスト名からWebUIを開く方法」](https://lemmy.juggler.jp/post/864/comment/562)
        -   \`{FQDN}/\_matrix/client/\`からサーバが用意するwebクライアントのリンクへのリダイレクト方法が書かれています。
-   Dendriteの構築
-   STUN/TURNサーバの構築
    -   [tateisuさんによるTURNサーバ構築メモ](https://lemmy.juggler.jp/post/769)

### Webクライアント構築

-   [Riotの構築]({{< relref "admins/install-riot" >}})

### Botの構築

-   [go-neb]({{< relref "admins/go-neb" >}})

## for Wiki contributor

-   Wikiの編集方法
