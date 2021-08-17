# Bridge

BridgeとはMatrixと他のプロトコル・サービスを相互接続するためのソフトウェア群のことです。Matrixは他のプロトコルとの相互運用性も重要視しています。Bridgeに対応しているサービスは以下の**参考文献**から参照することができます。

# Bridgeの概念

Bridgeにはいくつかの概念があります。

**Portal rooms**

ルームエイリアスの名前空間のチャンクを制御します。例えば"#freenode\_#channelname:matrix.org"はFreenodeの\"#channnelname\"に対応しています。このようにしてMatrixユーザーはFreenode上のIRCチャンネルに透過的に参加することができます。Portal
roomsは通常はリモートネットワーク側で管理されます。

**Plumbed rooms**

Plumbed
roomsはBridgeを設定することで、1つ以上の特定のリモートルームに「配管（plumbed）」されます。これは誰でも実行可能です。例えば、#matrix:matrix.orgはFreenode上の#matrix、Slack上のmatrixdotorg/#matrixなどに接続されています。Matrixユーザーのアクセス制御は、必然的にMatrix側で管理されます。Matrixを使って異なるコミュニティを連携させる場合に便利です。

**Bridgebot-style**

Bridgebot-styleでは、相互のプラットフォームからのメッセージが、指定されたプラットフォーム上に存在するボットによって伝えられます。これはメタデータが失われるため、最適な選択ではありません。具体的に説明すると、すべてのメッセージは同じボットによって送信されてしまいますが、メッセージのテキストの先頭には元の送信者の名前が付けられています。

**Puppeting**

Bridgeの反対側にいるユーザーを「人形化（puppeting）」することで、ボットベースのBridgingの問題を解決します。ネイティブユーザー視点では、メッセージが正しい送信者から送信されたものとして認識されます。ダブルパペット（double-puppeting）は、Bridgeの両方向でPuppetingが行われることを意味します。これは、Matrix
Bridgeを実装する際に最も好ましい方法です。

## 参考文献

-   [Bridges \| Matrix.org](https://matrix.org/bridges)
