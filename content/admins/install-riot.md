# Elementの構築

ElementはReact製のMatrixウェブクライアントで、Matrix-React-SDKによって開発されているようです。Element社というMatrixの開発を支援する企業が主体となって開発しています。

## Elementの更新

ElementはGitHubの公式リポジトリからアーカイブをダウンロードして解凍するだけで使用可能なので、更新スクリプトが様々な人ににょって作成されています。

-   [\@murz:ru-matrix.org](https://matrix.to/#/@murz:ru-matrix.org)
    氏作Bash製アップデートスクリプト
    <https://gist.github.com/MurzNN/ee64f98ab2e71b886c41d55594e5dd9e>
-   [さつき](https://matrix.to/#/@satsuki:iro.moe)
    氏作Bash製アップデートスクリプト <https://pastebin.com/PtYQmXSq>
    -   15行目
        `mv riot-* live`{.bash}を`mv element-* live`{.bash}へ修正することでElementへの対応が可能である。
