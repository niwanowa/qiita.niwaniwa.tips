---
title: Qiita CLIでできること、できないこと
tags:
  - Qiita
  - QiitaCLI
private: true
updated_at: '2023-11-15T15:52:31+09:00'
id: f8441d3b2183f2d4d67b
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
はじめまして。Qiita Advent Calendar 2023の完走賞を狙ってる[にわのわ](https://twitter.com/niwa_nowa)です。
Advent Calendarを走るにあたってローカルで記事の管理をしたいと思い導入しました。
手ごたえとしてはかなりおすすめです。

参考
> 完走賞
Qiita記事に1人で25記事投稿しきった方∞名を表彰します。（複数のカレンダーで計25記事の投稿も対象）

https://blog.qiita.com/adventcalendar-2023-qiitapresents/


# インストール
公式のインストールがしっかり書かれているのでそちらを参照してください。
自分のエディタで記事投稿ができる、Qiita CLIの使い方 #Qiita - Qiita

https://qiita.com/Qiita/items/666e190490d0af90a92b

# できること(うれしいこと)

普段は[こちら](https://hugo.niwanowa.tips/)でhugoを使ったブログを書いているので、そちらとの機能の比較をしていきます。
- ホットリロード機能
markdownファイルを保存すると自動でブラウザも更新してくれます。
無いと死んじゃう。
- サイドバーがとてもよい
非常に良くないですか？
良いですね。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/dc63649a-9039-f24b-1528-02e8b89aa419.png)
- ローカルでもQiitaの記法で表示される。
リンクカードなど、md標準でない機能もきちんと動いてくれます。感謝。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/c31a7b1b-9596-d941-0f1e-a1c55d9aa3fd.png)
- 改行が改行だけでよい
hugo、というかmdだと改行するときにスペース2つ入れないといけないのですが、Qiita CLIだと改行だけでよいです。
感情の赴くままにキーボードを叩く勢なので非常に助かります。日本語入力の時、特に。

# できないこと(ちょっと辛いこと)
- Qiita CLIの機能説明が公式説明で見つけられない
あったら申し訳ないのですが、ありますか？
インストールについてはめちゃめちゃ心遣いを感じるのに
こんな機能があるぜ！という説明が見つけられなかったので、最初の一歩に抵抗を感じました。
- テンプレートがない
```npx qiita new (記事のファイルのベース名)```で記事を作成すると、デフォルトのテンプレートが吐かれるのですが、
これを自分好みのものに変えられないです。
例えば```private: true```にするとか、やりたいことはあるのですが...
- ページバンドルがない
hugoにはpagebundleという機能があります。
これと[Paste Image](https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image)というVSCodeの拡張機能を使うと、ctrl+Vで画像を張り付けることができ、
画像の管理がしあわせに行うことができます。
要するにqiita CLIで画像を貼り付けようとするとちょっと辛いです。
Page bundles | Hugo

https://gohugo.io/content-management/page-bundles/

Paste Image - Visual Studio Marketplace

https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image

- リンクカードの表示がおかしくなる時がある
現状、再現2回。ctrl+S連打マンだとなるかも？

- updated_atが毎度更新される
限定共有記事限定？
特に差分がないはずなのに更新される。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/5b518e84-82e8-9bca-9824-a518ad11650f.png)

- タグに空白が含まれるとGHAで落ちる
これはQiitaにわかの自分が悪い部分もありますが、落ちてるときのログがこれ。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/46a84687-acc7-ed83-fa62-25f453866e9d.png)

# おまけ
こんな感じのtasks.jsonを書いておくとvscodeでQiita CLI用のプロジェクトを開くだけでプレビューページが表示されて幸せになれます。
```tasks.json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Qiita Preview",
            "type": "shell",
            "command": "bash -c \"npx qiita preview\"",
            "presentation": {
                "reveal": "always",
                "panel": "new"
            },
            "runOptions": {
                "runOn": "folderOpen"
            }
        }
    ]
}
```

# さいごに
以上。Qiita CLIに触れてみた感想でした。
マジで簡単に環境作れるのでおすすめです。
