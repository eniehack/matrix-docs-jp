# go-nebのインストール

ここではGolangの環境がないマシンにGolangの環境構築からgo-nebインストールまでを解説するページです。
想定OSはDebian
10です。そのため他OSの方は適宜読み替えるなどしてください。

## Golangの環境構築

Golangバージョン1.14以降が必要です。

    # apt install golang-go libsqlite3-dev

### 環境変数の編集

    export GOPATH=~/.go
    export PATH=$PATH:$GOPATH/bin

あくまでもGOPATHは慣例に習っただけなので自由に指定してもらって大丈夫です。

## インストール

### ビルド

    $ git clone https://github.com/matrix-org/go-neb
    $ cd go-neb
    $ go build github.com/matrix-org/go-neb

## 設定

### .serviceファイルの作成（optional）

## 実行

    $ sudo systemctl start go-neb

.serviceファイルを作成しなかった場合、実行時に環境変数を指定することで実行させることができます。以下が例です。

    $ BIND_ADDRESS=:4050 DATABASE_TYPE=sqlite3 DATABASE_URL=go-neb.db?_busy_timeout=5000 BASE_URL=https://example.com ./go-neb

## 関連ページ

## 参考文献

-   [go-neb bot Installation for Synapse-Matrix (arm64) -
    MyTinyDC](https://www.mytinydc.com/index.php/en/2019/08/04/go-neb-bot-installation-for-synapse-matrix-arm64/#page-content)
-   [matrix-org/go-neb: Extensible matrix bot written in
    Go](https://github.com/matrix-org/go-neb)
