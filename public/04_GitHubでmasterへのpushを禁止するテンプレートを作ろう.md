---
title: master(main)へのpushを禁止するテンプレートを作ろう
tags:
  - GitHub
private: true
updated_at: '2023-11-11T16:24:19+09:00'
id: 2cd18577c6ed3b0ecf96
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
gitを通ぶってジットと呼びます。嘘です。[にわのわ](https://twitter.com/niwa_nowa)です。

masterへ直pushし、もろもろのgithub actionsが走り、
runfailedメールを受け取ることがまれによくあります。
githubには[masterへの直pushを禁止する機能](https://docs.github.com/ja/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule)がありますが、
毎度設定するのはしんどいです。
ですのでテンプレート化しましよう。

# テンプレートリポジトリを作成する
githubには便利なことにテンプレートリポジトリという機能があります。
https://docs.github.com/ja/repositories/creating-and-managing-repositories/creating-a-template-repository

ということでトップページからぽちぽち作成。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/13f84bb2-9176-30ab-d480-1934f0c5ef04.png)

ついでなのでMITライセンスをデフォルトにするよ。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/ba2d0e6d-b38b-19b3-d314-b4ee52bed13d.png)

settings > General
Template repositoryにチェックを入れる。
![githubsettings.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/63fba93f-a3a0-f93d-c971-b5c1ccd9bf93.png)

# 直push禁止設定を入れる
上の方でリンクで紹介したこれはテンプレートに反映されないよ

https://docs.github.com/ja/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule

ということで、.githooks/pre-pushに設定を入れる。
```sh
#!/bin/bash

# pushを禁止するブランチ
readonly MASTER='main'
readonly RELEASE='^.*release.*$'  # 「release」文字列を含む全てのブランチ

while read local_ref local_sha1 remote_ref remote_sha1
do
  if [[ "${remote_ref##refs/heads/}" = $MASTER ]]; then
    echo -e "\033[0;32mDo not push to\033[m\033[1;34m master\033[m \033[0;32mbranch\033[m"
    exit 1
  fi
  if [[ "${remote_ref##refs/heads/}" =~ $RELEASE ]]; then
    echo -e "\033[0;32mDo not push to\033[m\033[1;34m release\033[m \033[0;32mbranch\033[m"
    exit 1
  fi
done
```

これを実行して完成 ~~本当はcloneするだけ、templateを選択するだけで直pushを禁止したかった~~
```
git config --local core.hooksPath .githooks
```

ok
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/a81902b5-3525-88ab-eece-88b68c9fc243.png)

~~参考~~まるパクリ

https://zenn.dev/aya_ryo/articles/c49f7606e928b0

# おわりに
.github/hooks/pre-push でよしなに
or
.git/hooksだけバージョン管理できるようにとかできないんですかね？
