---
title: Denoでプログレスバーを表示する
tags:
  - Deno
private: true
updated_at: '2023-11-11T23:24:58+09:00'
id: eee2846e9af9d699a1c3
organization_url_name: null
slide: false
ignorePublish: false
---

# はじめに

おにぎりくれる奴だいたい友達、[にわのわ](https://twitter.com/niwa_nowa)です。
本アドベントカレンダーのモチベーションを保つために、
アドベントカレンダーの進捗を表示するプログレスバーを作ることにしました。
プログレスバー、見てるとテンションアがるよね？

# 作ったもの
ソースはこちら

https://github.com/niwanowa/DenoAdventBar

![bar-kansei.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/a0e8eee9-ba6c-c596-8d35-63363f09b13b.gif)

# 要件

- ツイッターで呟きたいので1画面で収まるか、140字に収まること。
- 全体進捗と各記事の進捗を表示したい。
- 各記事の進捗は["タイトル未定", "タイトルだけ決まった" ,"書いてる","書いた"]の4段階で表現したい。

# 素材

もうライブラリあった。

https://deno.land/x/progress@v1.3.9

ということでREADMEにあったサンプルを動かしてみる
![bar-example.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/a2b27c0c-cb88-a653-daa0-9a28e7bdd953.gif)

いいじゃん！

# 作る

これから良い感じにカスタムしていきます。

同一ディレクトリにあるprogress.jsonを読みます。 jsonの形はこんな感じ。

```progress.json
"dayNN": {
        "tittle": "未定",
        "progress": "タイトル未定"
    },...
```

あとはサンプルコードをもとにプログレスバーの表示を行います

```main.ts
  function showProgress(progress: Progress[]) {
  if (completed <= total){
    completed += 1;
    bars.render(
      progress.map(p => ({ completed:(p.progress >= completed) ? completed:p.progress, total,  text: p.title }))
    )
    setTimeout(function () {
      showProgress(progress);
    }, 100);
  }else{
    bars.end();
  }
}
```
ポイントとしてはelseできちんとbars.end()を呼ぶことです。
でないとこんな感じで閉まらない終わり方になってしまいます。
![bar-kimoi.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/5280bdb1-de23-cebc-ceeb-b82da8bbd2a4.gif)

~~プログレスバーなのに完了しないことを想定してないからね…~~

# まとめ

プログレスバー、見てるとテンションアがるよね？
