---
title: この時代にRSSをホストする
tags:
  - 'RSS'
  - 'AWS'
  - 'python'
private: true
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
[にわのわ](https://twitter.com/niwa_nowa)です。
時代の逆光かはたまた先取りかRSS feedをAWSでホストしていこうと思います。

RSSというプロトコルを利用しつつも、特定のwebサイトの更新を取得するのではなく、
インターネットをプレイしている際に見つけた面白いサイトを自分のRSS feedに追加していきたい。というのが今回の目的です。

余談ですが、RSSは0.9x系だとRich Site Summary、1.x系だとRDF Site Summary、
2.0系だとReally Simple Syndicationの略語となっています。
ちなみに今回はAtomを使用します。

各バージョンについてはwikipediaがよくまとまっていると思います。

https://ja.wikipedia.org/wiki/RSS


# 作ったもの
今回作ったものはこちらで公開しています。

https://github.com/niwanowa/niwanowa-RSSFeed

AWS SAMで構築しているので
```bash
sam build
sam deploy -g
```
であなただけのRSSをホストできます。

# 仕様
## /
### GET
RSSFeedを取得する

中身
``` get.py
"""
get:
      summary: 'RSSFeedを取得するAPI'
      responses:
        '200':
          description: 'Successful response'
          content:
            application/xml:
              example: |
                <?xml version="1.0" encoding="UTF-8"?>
                <rss version="2.0">
                  <channel>
                    <title>Your RSS Feed</title>
                    <link>http://yourwebsite.com</link>
                    <description>Your feed description</description>
                  </channel>
                </rss>
      x-amazon-apigateway-integration:
        httpMethod: post
        type: aws_proxy
        uri:
          Fn::Sub: arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${GetRSSFeedFunction.Arn}/invocations
"""
import json
import boto3
import os

def lambda_handler(event, context):
    # S3クライアント作成
    s3 = boto3.client('s3')
    BUCKET_NAME = os.environ['BUCKET_NAME']

    # S3からrss.xmlを取得。
    # 対象ファイルがない場合はエラーを出力する
    try:
        response = s3.get_object(
            Bucket=BUCKET_NAME,
            Key='rss.xml'
        )
    except Exception as e:
        print(e)
        raise e
    
    # Lambdaプロキシ統合に対応したレスポンスを返す
    response = {
        "statusCode": 200,
        "headers": {
            "Content-Type": "application/xml"
        },
        "body": response['Body'].read().decode('utf-8')
    }

    return response
```

### POST
RSSfeedの追加を行う。
bodyにRSSFeedに追加したいURLを添える必要がある。
 ``` リクエストボディ例.json
 {
    "link": "https://qiita.com/niwanowa"
}
 ```

中身
``` post.py
"""
post:
      summary: 'RSSFeedを登録するAPI'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                link:
                  type: string
                  example: 'http://yourwebsite.com'
              required:
                - link
      responses:
        '200':
          description: 'Content added to the RSS feed'
"""

import json
import boto3
import os
import feedgenerator
import feedparser
from datetime import datetime, timezone
import requests
from bs4 import BeautifulSoup
import time

os.environ['TZ'] = "UTC"

def lambda_handler(event, context):
    print(event)
    # S3クライアント作成
    s3 = boto3.client('s3')

    # 環境変数からS3のバケット名とファイル名を取得
    BUCKET_NAME = os.environ['BUCKET_NAME']
    FILE_NAME = os.environ['FILE_NAME']

    # rssファイル読み込み
    obj = s3.get_object(Bucket=BUCKET_NAME, Key=FILE_NAME)
    rss_xml_string = obj['Body'].read().decode('utf-8')

    # feedparserでパース
    feed_parser = feedparser.parse(rss_xml_string)

    print(feed_parser)

    # FeedParserDictからfeedgeneratorのAtom用Feedオブジェクトを作成
    feed_generator = feedgenerator.Atom1Feed(
        title=feed_parser.feed.title,
        link=feed_parser.feed.link,
        description=feed_parser.feed.subtitle,
        language=feed_parser.feed.language,
        author_name=feed_parser.feed.author,
    )



    # Feedオブジェクトにパーサーのエントリーを追加
    for entry in feed_parser.entries:
        print(entry)
        feed_generator.add_item(
            title=entry.get('title'),
            link=entry.get('link'),
            description=entry.get('description', 'description is empty'),
            pubdate=datetime.fromtimestamp(time.mktime(entry.updated_parsed)),
        )


    print(feed_generator.writeString('utf-8'))

    # リクエストボディからlinkを取得
    link = json.loads(event['body']).get('link')

    # linkからページタイトルを取得
    page_title = get_page_title(link)

    # feedにエントリー追加
    feed_generator.add_item(
        title=page_title,
        link=link,
        description=page_title,
        pubdate=datetime.now(),
    )

    # Stringに書き出し
    rss_xml_string = feed_generator.writeString('utf-8')

    # #s3にアップロード
    s3.put_object(
        Bucket=BUCKET_NAME,
        Body=rss_xml_string.encode('utf-8'),
        Key=FILE_NAME,
        ContentType='application/xml'
    )


    # Lambdaプロキシ統合に対応したレスポンスを返す
    response = {
        "statusCode": 200,
        "headers": {
            "Content-Type": "application/json"
        },
        "body": json.dumps({"message": "Content added to the RSS feed"})
    }

    return response

def get_page_title(link):
    # ページタイトルを取得する。
    # ページタイトルが取得できない場合は、空文字を返す。

    try:
        response = requests.get(link)
        soup = BeautifulSoup(response.text, "html.parser")
        page_title = soup.title.string if soup.title else None
        return page_title
    except Exception as e:
      print(e)
      return None
```

## /reset
### GET
RSSFeedの初期化を行う。
ファイルがなければ作成し、ある場合は置換する。

中身
``` reset.py
"""
/reset:
    get:
      summary: 'RSSFeedをリセット(初回生成)するAPI'
      responses:
        '200':
          description: 'Initialized RSSFeed'
      x-amazon-apigateway-integration:
        httpMethod: post
        type: aws_proxy
        uri:
          Fn::Sub: arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${ResetRSSFeedFunction.Arn}/invocations
"""
import json
import boto3
import os
import feedgenerator
from datetime import datetime, timezone

def lambda_handler(event, context):
    # S3クライアント作成
    s3 = boto3.client('s3')

    # 環境変数からS3のバケット名とファイル名を取得
    BUCKET_NAME = os.environ['BUCKET_NAME']
    FILE_NAME = os.environ['FILE_NAME']

    # rssファイルの要素を初期化する。
    feed_title = "にわのわのRSS"
    feed_link = "https://example.com/rss"
    feed_description = "にわのわさんのRSSFeed。"
    feed_language = "ja"
    feed_author = "にわのわ"

    # エントリーの要素を変数で代入（例として2つのエントリーを生成）
    entry_title = "にわのわさんのブログ"
    entry_link = "https://hugo.niwanowa.tips/"
    entry_summary = "にわのわさんがやってるブログだよ。"

    # RSSフィードの基本構造を生成
    feed = feedgenerator.Atom1Feed(
        title=feed_title,
        link=feed_link,
        description=feed_description,
        language=feed_language,
        author_name=feed_author,
    )

    # フィードにエントリーを追加
    feed.add_item(
        title=entry_title,
        link=entry_link,
        description=entry_summary,
        pubdate=datetime.now(timezone.utc)
    )

    # RSSフィードのXMLを生成
    rss_xml_string = feed.writeString('utf-8')

    # S3のrss.xmlを置換する。
    s3.put_object(
        Bucket=BUCKET_NAME,
        Body=rss_xml_string.encode('utf-8'),
        Key=FILE_NAME,
        ContentType='application/xml'
    )

    # Lambdaプロキシ統合に対応したレスポンスを返す
    response = {
        "statusCode": 200,
        "headers": {
            "Content-Type": "application/json"
        },
        "body": json.dumps({"message": "Initialized RSSFeed"})
    }

    return response
```

# おわりに
「にわのわさんの良いコンテンツ」みたいな感じでインターネットをプレイしていた際に、
良いものを見つけたら気軽に共有していきたい。という思いで作りました。

