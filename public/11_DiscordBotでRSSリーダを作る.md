---
title: DiscordBotでRSSリーダを作る
tags:
  - 'python'
  - 'discord'
  - 'discord.py'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
車輪の再開発大好き、[にわのわ](https://twitter.com/niwa_nowa)です。

今日はDiscordBotでRSSリーダを作ってみました。
既存のDiscordアプリでRSSリーダはいくつかありますが、
サーバの雰囲気にあったアイコンを設定したいので自作しました。

例

https://rss.app/ja/bots/rssfeeds-discord-bot

https://monitorss.xyz/

# 作ったもの
今回作ったものはこちらで公開しています。

https://github.com/niwanowa/niwanowa-RSSReader


# ソース
これをEC2にデプロイして、常時起動させています。
DISCORD_TOKEN、CHANNEL_ID、RSS_URLは環境変数で設定しています。
それぞれほぼ読んで字の如くですが、
DISCORD_TOKENはDiscordのdeveloper portalから取得できます。
CHANNEL_IDはDiscordのチャンネルIDです。ブラウザでbotを置きたいチャンネルを開くいて取得します。

![名称未設定.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/76c2ed5b-7f0d-e418-b45c-054f0d3bf07f.png)

RSS_URLはRSSのURLです。```https://hogehoge.com/rss.xml```のような形式です。

```bot.py
import asyncio
import discord
from discord.ext import commands
from dotenv import load_dotenv
import os

import feedparser
from datetime import datetime,timedelta,timezone
import time


load_dotenv()

TOKEN = os.getenv('DISCORD_TOKEN')
CHANNEL_ID = int(os.getenv('CHANNEL_ID'))
RSS_URL = os.getenv('RSS_URL')
os.environ['TZ'] = "UTC"

intents = discord.Intents.default()
intents.guilds = True  # サーバーに関するイベントを取得できるようにする
intents.message_content = True # メッセージの内容を取得する権限

bot = commands.Bot(command_prefix='!', intents=intents)

@bot.event
async def on_ready():
    print(f'{bot.user} has connected to Discord!')

    # 参加しているサーバーのID一覧を取得
    guild_ids = [guild.id for guild in bot.guilds]
    print(f'Bot is in the following guilds: {guild_ids}')

    # サーバーIDを指定して、サーバーのオブジェクトを取得
    guild = bot.get_guild(guild_ids[0])
    print(f'Bot is in the following guild: {guild}')

    # サーバーのオブジェクトから、チャンネルのオブジェクトを取得
    await bot.get_channel(CHANNEL_ID).send('Bot has started.')

    # 1分おきに実行する
    while True:
        # RSSの取得
        feed = feedparser.parse(RSS_URL)
        print(f'Get RSS')
        await bot.loop.create_task(rss_task(feed))

        await asyncio.sleep(60)

# RSSの取得と送信
async def rss_task(feed):
        # RSSのentryを表示
    for entry in feed.entries:
        # updated_parsedが1分以内の場合はdiscordに送信
        pubdate=datetime.fromtimestamp(time.mktime(entry.updated_parsed), timezone.utc)
        five_minutes_ago = datetime.now(timezone.utc) - timedelta(minutes=1)

        print(f'pubdate: {pubdate}, five_minutes_ago: {five_minutes_ago}, pubdate > five_minutes_ago: {pubdate > five_minutes_ago}')
        if pubdate > five_minutes_ago:
            await bot.get_channel(CHANNEL_ID).send(entry.link)

bot.run(TOKEN)

```

# つまづいたところ
最初はdockerで動かそうと思っていましたが、Dockerコンテナ上で起動するとコネクションがうまく接続できませんでした。
インターネットを参照しても特別ポートを開いたりしてなさそうだったので、なんでかなぁという感じです。
諦めて今回はEC2にポン置きしています。

# おわりに
以上です。
DiscordBotはConnectionをずっと持ち続ける都合上、lambdaと相性が悪いと思ってEC2で動かしていまが、
今回のような使い方をする場合はlambdaでもいいかもしれません。
良きRSSライフを