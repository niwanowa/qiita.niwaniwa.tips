---
title: にわのわさんと振り返るOpenAI_DevDay
tags:
  - ''
private: true
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: true
---
# はじめに
[にわのわ](https://twitter.com/niwa_nowa)です。
2023/11/07に行われたOpenAI DevDayの内容について振り返っていこうと思います。
1ヶ月経って段々記憶が薄れてきたころあいだと思うので一緒に復習しましょう。

# 目線
- 普段はChat GPTを使って雑談することがメイン
- APIは初めて触る

# 発表されたこと
### GPT-4 Turbo（API）の発表
より高性能に、より安く使えるようになった。
APIなのでChatGPT Plusに入ってなくても触れる。

### Function callingのアップデート
1つのメッセージで複数の関数を呼び出せるようになった。

https://platform.openai.com/docs/guides/function-calling

### フォーマット指定時の安定性向上とJSONモードの登場
"XMLで出力して"みたいにフォーマットを指定したときにより従ってくれるようになったらしい。
新登場したModelの```gpt-4-1106-preview```と```gpt-3.5-turbo-1106```では
リクエストに```{ "type": "json_object" }```を付けることでJSON形式で必ず返してくれるようになった。

### seedの指定が可能に
requestにseedを設定することで再現性のある応答が可能になった。
> We at OpenAI have been using this feature internally for our own unit tests and have found it invaluable. 

とあるように単体テストで役に立ちそう。

### GPT-3.5 Turboのアップデート
```gpt-3.5-turbo-1106```ではGPT-4と同様に上に書かれた機能を利用できるようになった。
これもAPI、Chat GPT無料プランの方は更新されていないはず。

### Assistants APIの登場
APIでもカスタム指示を設定できるようになったってことでいいのかな？
webからも試せる

https://platform.openai.com/assistants

### 画像入力できるmodelが登場
```gpt-4-vision-preview```で画像入力が可能になった。
画像入力で別途課金されるので注意。

### DALL-E 3がAPIとして利用可能に
ChatGPT Plusで利用できていた画像生成AIがAPIとしても利用可能になった。

### Text-to-speech (TTS)APIが登場
英語に最適化されているが日本語でも利用可能。

# 試してみた

# 参考
New models and developer products announced at DevDay
https://openai.com/blog/new-models-and-developer-products-announced-at-devday

Introducing GPTs
https://openai.com/blog/introducing-gpts

OpenAI DevDayで発表された様々な機能について、公式ドキュメントを見ながら少しだけ詳細を確認してみた | DevelopersIO
https://dev.classmethod.jp/articles/openai-update-at-devday/

OpenAI Dev Dayで発表されたこと15選【プロンプトエンジニア必見 | 和訳 | まとめ】
https://zenn.dev/umi_mori/articles/openai-dev-day-1