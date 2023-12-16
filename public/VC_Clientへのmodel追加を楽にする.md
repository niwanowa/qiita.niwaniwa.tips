---
title: VC_Clientへのmodel追加を楽にする
tags:
  - Python
  - VCClient
private: true
updated_at: '2023-12-15T14:58:10+09:00'
id: 9162015962ef591c9b94
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
愚かな肉体人類のみなさん、こんにちは！[にわのわ](https://twitter.com/niwa_nowa)です。

私は肉体人類とのコミュニケーション手段としてAIボイスチェンジャーであるVC_Clientを使っています。
VC_Clientでは、モデルさえ追加も可能なのですが、まとめて複数のモデルを追加する際に画面操作で行うのは少し面倒でした。
そこでモデル追加を楽にするスクリプトを作ったので紹介します。

# VC_Clientとは
自分の声をリアルタイムに変換してくれるソフトウェアです。
自分は主にVALORANTで報告を聞こえやすくするために使っています。

https://github.com/w-okada/voice-changer

# ソース
以下のpythonファイルを```\MMVCServerSIO\model_dir\import.py```のような形で配置します。

```python
import os
import shutil
import sys
import json
import glob

# コマンドラインで指定したディレクトリを取得
# 指定がなかった場合はエラー出力して終了
if len(sys.argv) < 2:
    print("Please specify the directory to copy.")
    sys.exit(1)
source_dir = sys.argv[1]

# 現在のディレクトリにコピーする
# 既存の連番ディレクトリを確認し、次の番号を割り当てる
existing_dirs = [d for d in os.listdir('.') if os.path.isdir(d) and d.isdigit()]
next_dir_num = max([int(d) for d in existing_dirs] + [-1]) + 1
new_dir_name = str(next_dir_num)
shutil.copytree(source_dir, new_dir_name)

# params.jsonを作成
# コピーしたディレクトリ内の.pthファイルと.indexファイルの名前を取得
pth_file_name = glob.glob(f"{new_dir_name}/*.pth")[0]
index_file_name = glob.glob(f"{new_dir_name}/*.index")[0]

params = {
    "slotIndex": new_dir_name,
    "voiceChangerType": "RVC",
    "name": os.path.splitext(os.path.basename(pth_file_name))[0],
    "description": "",
    "credit": "",
    "termsOfUseUrl": "",
    "iconFile": "",
    "speakers": {"0": "target"},
    "modelFile": os.path.basename(pth_file_name),
    "indexFile": os.path.basename(index_file_name),
    "defaultTune": 12,
    "defaultIndexRatio": 0.4,
    "defaultProtect": 0.5,
    "isONNX": False,
    "modelType": "pyTorchRVCv2",
    "samplingRate": 40000,
    "f0": True,
    "embChannels": 768,
    "embOutputLayer": 12,
    "useFinalProj": False,
    "deprecated": False,
    "embedder": "hubert_base",
    "sampleId": "",
    "version": "v2"
}

with open(f"{new_dir_name}/params.json", 'w') as file:
    json.dump(params, file, indent=4)

print(f"Directory copied and params.json created in '{new_dir_name}'")

```

# 使い方
```bash
python3 import.py "対象のモデルが入っているディレクトリ"
```

これで次回実行時に追加したいモデルが読み込まれています。

# まとめ
今回はVC_Clientへのモデル追加を楽にするスクリプトを紹介しました。
良きバーチャルライフを！
