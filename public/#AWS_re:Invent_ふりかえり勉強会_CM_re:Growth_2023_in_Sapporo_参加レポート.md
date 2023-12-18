---
title: '#AWS_re:Invent_ふりかえり勉強会_CM_re:Growth_2023_in_Sapporo_参加レポート'
tags:
  - '参加レポート'
private: true
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: true
---
# はじめに
[にわのわ](https://twitter.com/niwa_nowa)です。


クラスメソッドさんが主催する、AWS re:Inventの振り返り勉強会に参加してきましたので、そのレポートです。

https://classmethod.connpass.com/event/303468/

札幌テレビ塔にレンタルルームあるんですね。

# セッション１ ： タイトル未定
reinventであったセッション
pythonを使ってたAPIの一部をRustで置き換えたらいい感じに早くなった

https://github.com/fun-with-serverless/rustifying-serverless

実際に検証してみた

ベンチマークツール
コールドスSAR-measure-cold-start
aws-lambda-power-tuning

# セッション２ ： reinvent2023を経て、AWS利用費に対するアプローチの変化

倹約アーキテクチャ
THE FRUGAL ARCHITECT
身の丈にあったコストをかけて常にコストをチェックしながらタイミングに応じてアーキテクチャを変更し、最適なコストを維持する

S3 Express １Zoneがでかかった

Resource Explorerのアップデート
AWSアカウント内で起動/設定されているリソースを検出、一覧化するるツール
今まで検索対象リソースが少なすぎて使い物にならなかった

AWS Organizationsの組織単位でのリソース検出

検索用フィルターが追加(単一タグだけで検索しづらかったリソースを検索できるようになる)
検索対象リソースが86リソース文のサポート

CWぉｇｓの低頻度アクセスログクラス(Infrequent Access)追加
なんでも感でもログ転送するとコストがかさむ
データの取り込みがかなり比重として高い

通常利用のログクラスに比べて半額ですみそう
すでに利用しているロググループの変更不可、明示的にロググループを変更しなければならない
サブスクリプションフィルターやGetLogEbentsが使用不可
特定のログ出力でLambdaキックしたりができない

メトリクスフィルターも使用不可
メトリクスを見てアラートを出すとかができない

純粋にログ保存のみなら

使い道
・メンテナンスによって出力されるログ
  ・codebuildやSSM操作ログなど
  SSMのログ出力できなかった
  切り替えるに当たって検証が必要かなと思った
・他サービスと絡まないログ
# セッション３ ： re:Invent 2023 コンテナセキュリティアップデートまとめ

Inspecter v2サポート

コンテナイメージスキャン
NIST SP 800-190
イメージのリスク
・イメージの脆弱性 ->イメージスキャン
・イメージの設定の不備->イメ0時の作り方 linterを使いながら
・埋め込まれたマルウェア->マルウェアスキャン
・埋め込まれた平文の秘密情報->Systems Managerを使ったり
・信頼できないイメージの使用 -> イメージの署名
レジストリのリスク
オーケストレーたのりすく
コンテナのリスク
ホストOSのリスク

イメージのスキャン
ECRベーシックスキャン
CICDに組み込むことができた

Inspector v2
頑張らないといけなかった
reinbetで簡単に組み込めるようになった
inspector-scanAPI AWS CLI,CDKで利用可能になった
SBOMファイルから脆弱性を検出

 inspector-sbomgen 
 SBOM作成ツール
 推奨スペック
 4x core cpu
 8gb ram

inspector v2 SBOM Export
Inspector v2で管理しているリソ=すのSBOM生成
Organizationsやろsp=すたんいでエクスポおと可能

出力形式が合わない
・CycloneDX１．４ 1.5が使いたい

AWS CLI v2 のバージョンを上げないと使えない

# セッション４ ： re:Invent 2023 ストレージサービスのアップデートを振り返る

S3 Express one zoneストレージクラスの発表
シングルAZ
レイテンシー重視、高頻度アクセス向け
特に小さなオブジェクトに対してアクセスが多いときに効果的なストレージクラス
ディレクトリバケットと呼ばれる
AWS CLI、S3APIは今までと同じように使える

PUT、COPYのリクエスト料金は標準に比べ半額
保存単価は7倍
長期保存には向かないか

ユースケース
AIMLトレーニング、HPC(スパコン)を使った並列処理演算などで大量のデータ処理
Athenaからのクエリパフォーマンス改善。最大2,1倍高速化
S3標準は段階的なスケーリング
OneZone瞬時

データはシングルAZ保管
シングルAZ内で冗長化される

AZをまたぐ通信料は発生しない

IAMポリシーの書き方に注意
S3とS3 Expressで名前空間が異なる
AmazonS3FullAccessでもアクセスできない

Gateway型のVPCエンドポイントは別に必要
S3のよく知られている機能はだいたい利用できない
バージョニング、VPCエンドポイント

AWS Backup非対応

ハイパフォーマンス向け

Amazon EFSのアップデート
保存単価の安いストレージクラス追加
保存単価は低頻度アクセスストレージクラスの半額
ライフサイクル管理の設定必要
標準 0.36$
低頻度アクセス $0.02
アーカイブ $0.01

ユースケース
年に数回しかアクセスしないデータ
レイテンシは低頻度アクセスと同様

使えるのはElsticスループットモードだけ
ファイルシステムタイプ リージョナル スループットモード Elastic ストレージクラス アーカイブ
のパターンのみ
渋滞からあるBurstingだと使えない
IOPSが上がったのはElsticスループットモードのみ
低頻度アクセスクラスが値下げされたけどリージョナルタイプのみ

まとめ
S3に7つ目の新しいストレージクラスが追加された
EFSに新しいストレージクラスとしてアーカイブが追加された

# セッション５ ： re:Invent2023 Analytics関連アップデート
Tablowに近いUIになった

Amazon Redshiftのアップデート
Zero-ETL
Redshiftから書くDBのデータにアクセス、ほぼリアルライムの分析が可能
DynamoDBから Opensearch ServiceとのZero-ETLが可能に
AIドリブンなスケーリングと最適あ機能がプレビューで利用可能に
Amazon Q generative SQLがQueryEditorV2で利用可能に
Amazon RedshigyクエリエディタがAmazonb Q generative SQLの新機能として発表
データ共有が複数のDWHからの書き込みクエリをサポート
読み取りだけだったのが書き込みも可能に

Amazon Athenaのアップデート
S3 Express one zoneによるクエリを高速化
S3 Standardより最大10倍優れたパフォーマンス

QuickSight Q in QuickSightを発表
生成BI機能をさらに拡張した

AWS Glueのアップデート
Amazon Qの統合
自然言語でGlueによるデータパイプラインを生成

Amazon Q
生成AIアシスタント

# セッション６ ： 開発系アップデート、 AppSync周りなどまとめ
AppSyncアップデートまとめ

GraphQL
REST以外の選択肢としてGraphQLも頭の片隅に入れておく

GrapgQL（Appsync）振替地
ResolverがGraphQLから書くDB向けに変換してくれる

GraphQLの嬉しいところ
APIをGraphQLに寄せることでクライアントから柔軟にデータベースを読み書きできる

AppSyncを使うメリット
自前で裏側のResolverを動かすフレームワークを考えずとも、いい感じにGraphQLを使える
DynamoDB Resolverなど、Amplifyとセットで使うと感動するらしい

RDS Data APIを利用して DB SchemaをもとにGraphQL APIが作成できるようになった
RDS Data API経由ですでにあるテーブルをもとに自動で色々作ってくれる
RDS Data PIに対応しているのは Aurora Serverless v1のみ
 -> MySQL5.7

AppSyncとどうやって向き合っていけばいいか
DynamoDBでデータベースを単純にフロントから読み書きしたい場合

Amplify ＋ AppSync ＋ DynamoDB Resolverやってみたい

# セッション７ ： 量子コンピューティング系の何か
Amazon Braket SDK
現状は物理実験をやる感覚
開発->実験をスムーズに行き来できる
OSS

Amazon Braket Direct
専有利用できるサービスが登場
待ち時間無しに利用可能
ハードウェアの調整が可能
最新世代ハードウェアの利用

Amazon Braket Digital Badge
トレーニングコースと試験の提供開始
・無料
・体系的に学べる

電子計算の状況
物理実験をやる感覚
実際に動かせるの
アルゴリズムやデバイスの研究・学習用
まだまだ規模を拡大する必要あり

物理と情報のレイヤを跨ぐエキサイティングな分野

# セッション８ ： re:Invent 2023 サーバーレスアップデートまとめ

「サーバーレスアップデートまとめ」というタイトルで CM re:Growth 2023 in Sapporo に登壇しました #AWSreInvent #cmregrowth | DevelopersIO
https://dev.classmethod.jp/articles/regrowth2023sapporo-serverless/

Amazon InspectorのLambdaコードスキャンが生成AIを活用して修復コードを提示してくれるようになった

StepFunctionsから直接パブリックHTTPエンドポイントを実行できるようになった
サードパーティのAPI呼び出しなど
ワークフローでエラーハンドリング機能やジッターなど使える
EventBrigheのAPI送信先接続機能などもある

Amazon Bedrockように最適化された２つの新しいAPIアクションがAWS StepFunctionsに追加された
アップデート前はLambda必要だった
InbokeModelとCreateModelCustomizeJobができるように

Amazon EventBrisgeのSaaS統合でAdobeとStripeがサポートされるようになった
Stripeはベータ申請が必要

Amazon SQS DLQのりドライブ先に

ElasticCache ServerlessがGA
従来型クラスターより高速にプロビジョニング
キャパシティプランニング不要

トラフィックが予想できない、波があるときに使うもの
予想できるなら従来型を使う

Application Composer
VSCode拡張機能でビジュアルエディタを直接触れるようになった

Aurora Limitless Database
マネージドなシャードグループで書き込みスケールを実現

Amazon Managed Blockchain
サーバーレスパブリックネットワークでPolukadotがサポートされた

Breakout Sessionを確認おすすめ

# セッション９ ： 盛りだくさんすぎるAI系アップデートを時間の許す限り紹介しますre:Invent 2023 で発表された新サービスのまとめ

生成AIのスタックは３走行性
学習のためのインフラ基盤
Trainiumチップ、EC2、sagemaker

FM活用のためのルーツ
Bedrock

FMを使ったアプリケーション
Amazon Q、CodeWhisper

Vector Serarch関連アップデート
生成系AIでは入力されるテキスト・画像・音声などを数値で表現する
ベクトル表現（エンベディング）で取り扱う

Vector engine for OpenSearch Serverlessが一般利用開始に
シンプルで高性能なベクトルDB
LangChainなどのオープンソースツールと互換性あり
ベクトル検索とフルテキスト検索ができる

Amazon Document DB、Amazon DynamoDBがベクトル検索に対応

Amazon Q関連のアップデート
新しいタイプの生成系AIによるアシスタント

AmazonQは主に３種類
Amazon Q
  まねこんの横についてるやつ
Amazon Q for business use
  RAG 検索などができるやつ
Amazon Q in XXX
  ほかサービスに自然言語でクエリをかけられるやつ

Amazon Q for business use
  RAGアプリケーションを簡単に構築できる
  裏ではKendraが動いている
  繋げられるデータソースがやたらと多い
  webクローラーもいる

Amazon Bedrock knowledge Base
  裏で、Vector engine for OpenSearch Serverlessが作成される
  モデルがクロウド

Agent for Amazon Bedrock
  Lambda関数を定義してAgentを作成
  エージェントはユーザリクエストを分析、基盤モデルの推論機能によりリクエストを完了するための処理順序を決定する

日本語がちょっと苦手

SageMaker Canvas
