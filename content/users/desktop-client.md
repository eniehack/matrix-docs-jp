## デスクトップクライアント

### Riot

[Web版Riot](web-client#)を[Electron](https://ja.wikipedia.org/wiki/Electron_(%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2))という技術によりデスクトップアプリケーションとして動くようにしたもの。実はWeb版Riotとコードは変わらない。

  -------------- -------------------------------------------------
  公式サイト     <https://riot.im>
  作者           Vector.im
  開発言語       Javascript(React)
  ソースコード   [GitHub](https://github.com/vector-im/riot-web)
  動作環境       Electron(Windows/MacOS/Linux)
  ダウンロード   <https://riot.im/download/desktop/>
  -------------- -------------------------------------------------

### Fractal

Unix系のデスクトップ環境や、GUIソフトウェアを開発する団体であるGNOMEが開発するデスクトップクライアント。Flatpakによる配布。ArchLinuxユーザは[公式repo](https://www.archlinux.org/packages/community/x86_64/fractal/)からも入手可能。

  -------------- --------------------------------------------------------
  公式サイト     <https://wiki.gnome.org/Apps/Fractal>
  作者           GNOME
  開発言語       GTK,Rust
  ソースコード   [GNOME GitLab](https://gitlab.gnome.org/GNOME/fractal)
  動作環境       Linux(Unix?)
  ダウンロード   <https://wiki.gnome.org/Apps/Fractal>
  -------------- --------------------------------------------------------

### NeoChat

Unix系のデスクトップ環境や、GUIソフトウェアを開発する団体であるKDEが、開発するマルチプラットフォームなクライアント。
安定版(stable)としてリリースされているのはLinux Flatpakのみ。
しかし、nightly版としてWindows、MacOS、Android、Linux
AppImageも配布されている([詳細](https://invent.kde.org/network/neochat#get-it))。

  -------------- ------------------------------------------
  公式サイト     <https://apps.kde.org/en/neochat>
  作者           KDE
  開発言語       Qt,C++,QML
  ソースコード   <https://invent.kde.org/network/neochat>
  動作環境       Linux
  ダウンロード   <https://wiki.gnome.org/Apps/Fractal>
  -------------- ------------------------------------------

### Nheko

Qtで開発されたマルチプラットフォームなデスクトップクライアント。End-to-End
EncryptionやVoIPにも対応。
ちなみに、Nheko開発チームはC++(Boost.Asio)製のMatrixライブラリ[mtxclient](https://github.com/Nheko-Reborn/mtxclient)も開発している。

  -------------- --------------------------------------------------
  公式サイト     <https://nheko-reborn.github.io/>
  作者           [Nheko-Reborn](https://github.com/Nheko-Reborn)
  開発言語       Qt,C++,QML
  ソースコード   <https://github.com/Nheko-Reborn/nheko>
  動作環境       Linux,Windows,MacOS
  ダウンロード   <https://github.com/Nheko-Reborn/nheko/releases>
  -------------- --------------------------------------------------

### WeeChat-Matrix

コンソールで動作する、拡張が容易なIRCクライアント[WeeChat](https://weechat.org/)のプラグインとして、動作するクライアント。
Rustでリライトした、開発中の[weechat-matrix-rs](https://github.com/poljar/weechat-matrix-rs)も存在する。

  -------------- --------------------------------------------
  公式サイト     <https://github.com/poljar/weechat-matrix>
  作者           [poljar](https://github.com/poljar)
  開発言語       Python
  ソースコード   <https://github.com/poljar/weechat-matrix>
  動作環境       WeeChat（Unix系)
  ダウンロード   <https://github.com/poljar/weechat-matrix>
  -------------- --------------------------------------------
