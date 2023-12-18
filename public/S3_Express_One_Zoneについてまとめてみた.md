---
title: S3 Express One Zoneについてまとめてみた
tags:
  - AWS
  - S3
private: true
updated_at: '2023-12-18T14:45:54+09:00'
id: e404e30666b98dd95315
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
[にわのわ](https://twitter.com/niwa_nowa)です。

# S3 Express One Zoneとは
低レイテンシーで高頻度アクセス向けのストレージクラスです。
大本営発表で**最大10倍**のパフォーマンスを発揮すると言われています。

# 値段の比較
保存料金はS3 Standardの約7倍、データアクセス料金はS3 Standardの約半分です。
短いライフサイクルの中で小さなオブジェクトに対してアクセスが多いときにより効果的です。

### 保存料金
S3 Standard：$0.025 per GB
S3 Express One Zone：$0.18 per GB

### データアクセス料金
S3 Standard：$0.0047	$0.00037
S3 Express One Zone：$0.0024	$0.00019

参照

https://aws.amazon.com/s3/pricing/?nc1=h_ls

# 作り方
こちらを参照

https://aws.amazon.com/jp/blogs/news/new-amazon-s3-express-one-zone-high-performance-storage-class/

# アクセス方法
AWS CLIやS3 APIから通常のS3と同様に利用できます。

ただし、IAMポリシーの書き方に注意が必要で、
アクション名が```s3express:*```となります。

https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-express-security-iam.html

# おわりに
re:Inventで発表されたS3 Express One Zoneについてまとめてみました。
特にDWH辺りとの連携となってくると使う機会があると思うので、頭の片隅に残せられればと思います。
