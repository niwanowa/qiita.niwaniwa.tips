---
title: 短縮URLを自作する
tags:
  - Python
private: true
updated_at: '2023-12-13T21:44:53+09:00'
id: d8136c5f476f8596288d
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
短縮URL、使っていますか？[にわのわ](https://twitter.com/niwa_nowa)です。

自分は端末間でURLを共有するときに、短縮URLを使うことが多いです。
しかし、短縮URLを作るサービスはたくさんありますが、自分にとってとても難しい問題があります。
それは自分にはURLが覚えられないということです。

参考：
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/94251bb2-d4d2-dc28-4e33-04601d91244e.png)

ということで。今回は以下の要件で短縮URLを作るサービスを作ってみました。
- （主に）自分のみが使う前提
- URLは数字のみにして覚えやすいものにする

# 今回みなかったことにしたもの
OSSで短縮URLをホストできるもの
自分で作ることにも意味があったと思っています

https://polrproject.org/

https://yourls.org/

# 作ったもの
今回作ったものはのソースはこちらで公開しています。

https://github.com/niwanowa/omit_url

# ソース
以下の2つのファイルをlambdaにアップロードして使います。
```登録用.py
import json
import boto3

def lambda_handler(event, context):

    table_name = "omit_url"

    #リクエスト情報のログ出力 
    print("===event===")
    print(event)

    print("===context===")
    print(context)

    # eventからurlを取得
    url = json.loads(event.get('body')).get('url')
    print("===url===")
    print(url)

    # DynamoDBからid(プライマリキー)の最大値を取得
    client = boto3.client('dynamodb')
    response = client.scan(
        TableName=table_name,
        Select='COUNT'
    )

    print("===response===")
    print(response)

    count = int(response.get('Count'))+1

    # DynamoDBにデータを登録
    response = client.put_item(
        TableName=table_name,
        Item={
            'id': {
                'N': str(count)
            },
            'url': {
                'S': url
            }
        }
    )

    # CORS対応
    headers = {
        'Access-Control-Allow-Headers': 'Content-Type',
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Methods': 'OPTIONS,POST,GET'
    }

    return {
        'statusCode': 200,
        'headers': headers,
        'body': json.dumps(count)
    }
```

```リダイレクト用.py
import json
import boto3
import os

def lambda_handler(event, context):

    table_name = "omit_url"

    #リクエスト情報のログ出力 
    print("===event===")
    print(event)

    print("===context===")
    print(context)

    # eventからidを取得
    id = int(event.get('pathParameters').get('id'))

    # DynamoDBからidをキーにしてデータを取得
    client = boto3.client('dynamodb')
    response = client.get_item(
        TableName=table_name,
        Key={
            'id': {
                'N': str(id)
            }
        }
    )

    print("===response===")
    print(response)

    url = response.get('Item').get('url').get('S')

    # urlへリダイレクトするhtmlを生成する
    html = f"""
    <html>
    <head>
    <meta http-equiv="refresh" content="0;URL={url}">
    </head>
    <body>
    </body>
    </html>
    """

    return {
        'statusCode': 200,
        "headers": {"Content-Type": "text/html"},
        'body': html
    }
```
# おわりに
今回は短縮URLをホストするサービスを作ってみました。
作ってから少し時間が経っていますが、そこそこ使っているので満足さんです。
