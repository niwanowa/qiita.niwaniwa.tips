---
title: AWS SAM入門してみた
tags:
  - AWS
  - AWSSAM
private: true
updated_at: '2023-11-14T22:41:29+09:00'
id: c6c4dbd47bbf00a138f3
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
jenkinsおじさんになりたくないって言いながらIaCらへんから逃げてきた[にわのわ](https://twitter.com/niwa_nowa)です。
筋トレするとたちまちムキムキになるわけないのと一緒で
IaCできるからってjenkinsおじさんになるわけではないですからね。
やりましょう。

ちなみに一番好きなjenkinsおじさんはこれ
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/7d709425-2dec-8827-ddf5-743aba373f00.png)

# 今回やること
デベロッパーガイドを読んで、解釈してHello Worldすることをゴールとします。

https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/what-is-sam.html

# AWS Serverless Application Model (AWS SAM) とは
> AWS Serverless Application Model (AWS SAM) は、AWS 上でサーバーレスアプリケーションを構築および実行するデベロッパーのエクスペリエンスを改善するツールキットです。

ふむ、なるほど。
これを使うとコマンドを叩くとよしなにAPI Gatewayとかlambdaとかを構成してくれるはず。

# インストール
これまたデベロッパーガイドを元にインストールしていきます。

https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/install-sam-cli.html

> 2023 年 9 月以降、AWS では AWS で管理される AWS SAM CLI (aws/tap/aws-sam-cli) 用 Homebrew インストーラーのメンテナンスを行いません。

とのことなので、```brew install```は使いません。

pkgファイルをダウンロードして以下のコマンドを叩きました。
``` bash
sudo installer -pkg ~/Downloads/aws-sam-cli-macos-x86_64.pkg -target / 
```
こんな感じ
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/44a244fc-ca23-2e2c-685c-d7368f75e8bf.png)

ガイドさんは親切なので動作確認まで書いてくれてます。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/e03e6c8e-4157-3f17-84d6-67d980812150.png)

# Hello World
例によってデベロッパーガイドを元にやっていきます。

https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/serverless-getting-started-hello-world.html

## ステップ 1: サンプルの Hello World アプリケーションを初期化する

```sam init```以降の入力を参考までに残します。
``` bash
~/D/aws_sam_hello_world ❯❯❯ sam init

You can preselect a particular runtime or package type when using the `sam init` experience.
Call `sam init --help` to learn more.

Which template source would you like to use?
        1 - AWS Quick Start Templates
        2 - Custom Template Location
Choice: 
```

1を入力

``` bash
Choose an AWS Quick Start application template
        1 - Hello World Example
        2 - Data processing
        3 - Hello World Example with Powertools for AWS Lambda
        4 - Multi-step workflow
        5 - Scheduled task
        6 - Standalone function
        7 - Serverless API
        8 - Infrastructure event management
        9 - Lambda Response Streaming
        10 - Serverless Connector Hello World Example
        11 - Multi-step workflow with Connectors
        12 - GraphQLApi Hello World Example
        13 - Full Stack
        14 - Lambda EFS example
        15 - Hello World Example With Powertools for AWS Lambda
        16 - DynamoDB Example
        17 - Machine Learning
Template: 
```

1を入力

```bash
Use the most popular runtime and package type? (Python and zip) [y/N]: 
```

yを入力

```bash
Would you like to enable X-Ray tracing on the function(s) in your application?  [y/N]: 

Would you like to enable monitoring using CloudWatch Application Insights?
For more info, please view https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-application-insights.html [y/N]: 

Project name [sam-app]:
```
３回ほどエンターキー連打(デフォルトでよい)

```bash
Cloning from https://github.com/aws/aws-sam-cli-app-templates (process may take a moment)                                                  

    -----------------------
    Generating application:
    -----------------------
    Name: sam-app
    Runtime: python3.9
    Architectures: x86_64
    Dependency Manager: pip
    Application Template: hello-world
    Output Directory: .
    Configuration file: sam-app/samconfig.toml
    
    Next steps can be found in the README file at sam-app/README.md
        

Commands you can use next
=========================
[*] Create pipeline: cd sam-app && sam pipeline init --bootstrap
[*] Validate SAM template: cd sam-app && sam validate
[*] Test Function in the Cloud: cd sam-app && sam sync --stack-name {stack-name} --watch
```

こんな感じでできました。

## ステップ 2: アプリケーションを構築する
```sam build```を叩くとlambdaへデプロイするようにディレクトリを作ってくれたり、
関連するcfnテンプレートを作ってくれるっぽいです。


```bash
cd sam-app
```

```bash
sam build
```

## ステップ 7: (オプション) アプリケーションをローカルでテストする

数字が飛びました。
本来のステップ3はデプロイなのですが、ローカルで動かすほうが先でしょ、ということでこの順番にしています。

以下のコマンドで実行ができます。
app.pyのlambda_handlerが呼び出されます。
```bash
sam local invoke
```

```--event```オプションでイベントを渡すことができることと、
```--env-vars```オプションで環境変数が設定できることは覚えておくと役立つかもです。

以下のコマンドでlambdaのホストができます。
こっちがメインで使いそう
```bash
sam local start-api
```
ちょっとcurlが文句言ってますがうまく動いてますね。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/29c42fc3-302b-234a-23db-a37feb0d3ef2.png)

## ステップ 3: アプリケーションを AWS クラウド にデプロイする
きちんと動くことが確認出たのでデプロイしていきます。
```bash
sam deploy --guided
```
チュートリアルでは以下の要素がデフォルトとは異なった設定となっているので注意してください。
```bash
 Confirm changes before deploy [Y/n]: n
 ```

 ```bash
  HelloWorldFunction may not have authorization defined, Is this okay? [y/N]: y
 ```

以下のように表示されれば成功
```bash
Successfully created/updated stack - sam-app in ap-northeast-1
```

早速アクセスしてみるとうまくデプロイできているっぽいです。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/10c7533a-46f7-0553-46d8-2eee54658fe0.png)

マネコンからも確認できました。

Amazon API Gateway
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/7ec7823c-2a59-edf7-8a97-2d72c678df17.png)

AWS Lambda
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/8b5491d2-7e74-bd55-a832-51f35661b079.png)

# おわりに
AWS SAMを使ってHello Worldをデプロイするところまでやってみました。
思わず僕もサムズアップ(これが言いたかった)
