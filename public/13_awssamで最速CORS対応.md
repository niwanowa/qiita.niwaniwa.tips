---
title: AWS SAMで最速CORS対応
tags:
  - AWS
  - AWSSAM
private: true
updated_at: '2023-12-10T15:59:14+09:00'
id: 5e9a0b93e56438edff63
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
丁寧なインターネット生活を。[にわのわ](https://twitter.com/niwa_nowa)です。

前回、Chrome拡張でPOSTリクエストを送信するものを作りましたが、
fetchで送信するとCORSエラーが発生してしまいました。

API Gatewayの設定を変更してこれを解消していきます。
個人で開発しているものかつ、URLを公開していないのでとにかくCORS対応して動作確認をしたい。
という視点での設定となっています。

# 作ったもの
CORS対応を行ったコミットはこちらです

https://github.com/niwanowa/niwanowa-RSSFeed/commit/79c841b360baeebccc5b0f161561e027f2bac00c

```template.yaml

Type: AWS::Serverless::Api
    Properties:
      StageName: Prod
      Cors: 
        AllowOrigin: "'*'"
        AllowHeaders: "'*'"
        AllowMethods: "'*'"
...

```
 のように設定を記載します。

```Cors: "'*'"``` という記載では動作しませんでした。

# おわりに
AWS SAMはかなり使い勝手が良いですが、少しニッチでドキュメントが少なく感じます。
ですのでどんなひとくちナレッジでも記事にしようと思います。

# 参考
https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/sam-property-api-corsconfiguration.html
