---
title: Denoでプログレスバーを表示する
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
おにぎりくれる奴だいたい友達、[にわのわ](https://twitter.com/niwa_nowa)です。
本アドベントカレンダーのモチベーションを保つために、
アドベントカレンダーの進捗を表示するプログレスバーを作ることにしました。
プログレスバー、見てるとテンションアがるよね？

# 作ったもの

# 要件
- ツイッターで呟きたいので1画面で収まるか、140字に収まること。
- 全体進捗と各記事の進捗を表示したい。
- 各記事の進捗は["タイトル未定", "タイトルだけ決まった" ,"書いてる", "書いた"]の3段階で表現したい。

# 素材
もうライブラリあったやん
progressbar@v0.2.0 | Deno

https://deno.land/x/progressbar@v0.2.0

ということでこう
同一ディレクトリにあるprogress.jsonを読みます。
```deno
```

# まとめ
プログレスバー、見てるとテンションアがるよね？