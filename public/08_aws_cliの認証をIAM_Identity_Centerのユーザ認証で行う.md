---
title: AWS CLIの認証をIAM Identity Centerのユーザ認証で行う
tags:
  - AWS
  - aws-cli
  - awscli
private: true
updated_at: '2023-12-02T20:06:28+09:00'
id: 20dd58d576b81dad7d7a
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
丁寧なインターネット生活を、[にわのわ](https://twitter.com/niwa_nowa)です。

新しくパソコンをセットアップしていく過程でいつものようにアクセスキーを登録しようとしたら、
こんな画面に出会いました。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/ac084271-6c59-6c55-41a4-36576b418a6d.png)

> AWS CLI V2 を使用し、IAM Identity Center のユーザーによる認証を有効にします。

これを試していこうと思います。

# SSO トークンプロバイダー設定
AWS IAM Identity Centerを使用して CLIでユーザ認証を行うには方法は以下の2つだそうです。
特にこだわりはないので推奨された方法で設定していきます。

>(推奨) SSO トークンプロバイダー設定。SSO トークンプロバイダー設定、AWS SDK、またはツールは、更新された認証トークンを自動的に取得できます。
> 更新不可のレガシー設定。更新不可のレガシー設定を使用する場合、トークンは定期的に期限切れになるため、手動で更新する必要があります。

## IAM Identity Centerの設定
ダッシュボードからユーザーを追加していきます。
**操作はバージニア北部(us-east-1)で実施してください。**

<details><summary>こうなった</summary><div>

東京リージョンでIAM Identity Centerを有効化すると
許可セットやAWSアカウントの選択ができなくなる
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/31267606-430b-63ca-d632-ab174cad3bc7.png)
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/da79c04a-c98c-a4e8-4b71-e20211532f2b.png)

</div></details>

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/37728b50-314e-8fa3-7593-a6908889a1a7.png)

### ユーザーを追加
入力が必要な項目は以下です。
ユーザー名 : よしなに
パスワード : パスワードの設定手順が記載された E メールをこのユーザーに送信します。にチェック
Eメールアドレス : よしなに
Eメールアドレスを確認 : よしなに
名 : よしなに
姓 : よしなに
表示名 : よしなに
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/a35bcbbb-5d06-632f-f01f-8a800304dee9.png)

お問い合わせ方法以降の設定はデフォルトのままとしました。

### グループ追加
今回はそのまま次へを押してグループは作成しませんでした。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/67b2c0a5-7204-9bd2-717b-565f47c70262.png)

### ユーザーの確認と追加
今まで入力してきた項目が合っているか確認します。
問題なければ次へ
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/61ec1e51-1afd-35f3-4d2b-e6f09aaf41e9.png)

### ユーザーの有効化
先ほど入力したメールアドレス宛によくあるタイプのメールが届いています。
```Accept invitation```をクリックしてユーザーを有効化します。

こんな感じの画面に飛ぶのでパスワードを設定
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/cfb7a25a-9071-bcd9-5529-e91fb319c213.png)

設定が終わったら再度ログインしてMFAを設定します。
MFAは各々で設定してください。

### 許可セットを作成
ダッシュボードから許可セットを作成します。
マルチアカウント許可 -> 許可セット -> 許可セットを作成
以下を設定して次へ
- 許可セットのタイプ ： 事前定義された許可セット
- 事前定義された許可セットのポリシー：AdministratorAccess

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/413a73fd-d472-888b-8544-cac17b4937e8.png)

セッション期間をよしなに変更して次へ
(自分の場合は８時間にしました。)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/1d6b6403-2bd9-fc05-31ae-f1e7f2098d1d.png)

確認して次へ

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/6fca0b2f-1a40-2013-63c4-bd670e790962.png)

### 許可セットの割り当て
作成した許可セットを割り当てます。
マルチアカウント許可 -> AWSアカウント -> ユーザーまたはグループを割り当て

1. 先程作成したユーザーを選択して次へ
2. 先ほど作成した許可セットを選択して次へ
3. 確認して送信

# AWS CLIの設定
次にAWS CLIの設定を行います。

```bash
aws configure sso
```

- SSO session name (Recommended): my-sso
- SSO start URL [None]: https://my-sso-portal.awsapps.com/start
URLはダッシュボードから確認できる
![名称未設定2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/1df6c747-4571-136e-30e7-679f5abb9bcc.png)
- SSO region [None]: us-east-1
- SSO registration scopes [None]: sso:account:access

ブラウザが立ち上がるので、ログインを行う。
以下のように表示されればOK。
```bash
The only AWS account available to you is: hogehoge
Using the account ID hogehoge
The only role available to you is: AdministratorAccess
Using the role name "AdministratorAccess"
```

続いてCLIの設定を行います。
profile nameをデフォルトにすることで優勝できます。
```bash
CLI default client Region [None]: ap-northeast-1
CLI default output format [None]: yaml-stream
CLI profile name [123456789011_ReadOnly]: default
```

最後に```aws s3 ls ```を実行してうまく動作すれば終了です。

# おわりに
東京リージョンでIAM Identity Centerを有効化すると云々、他のサービスでもハマりそう...

# 参考

https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/cli-configure-sso.html

https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html
