---
title: にわのわさんと振り返るOpenAI_DevDay
tags:
  - 'OpenAI'
  - 'ChatGPT'
private: true
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
どうもOpenAIのAPIをOpenAPIと書きがちな[にわのわ](https://twitter.com/niwa_nowa)です。
2023/11/07に行われたOpenAI DevDayの内容について振り返っていこうと思います。
1ヶ月経って記憶が薄れてきた頃合いだと思うので一緒に復習しましょう。

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
### 実行環境
全体のソースはこちら

https://github.com/niwanowa/tryOpenAIAPI

python 3.10
共通設定として以下を実行しています。
```bash
poetry init -n
poetry add python-dotenv
poetry add openai
```

### GPT-4 Turbo
```python
from openai import OpenAI
from dotenv import load_dotenv
import os
load_dotenv()

client = OpenAI(
    api_key=os.getenv("OPEN_AI_KEY"),
)

response = client.chat.completions.create(
    model="gpt-4-1106-preview",
    messages=[
        {"role": "system", "content": "新興宗教コンサルティングのスペシャリストです。"},
        {"role": "user", "content": "ITエンジニアのための宗教を作ろうと思います。テックなネタに絡めたユーモラスな宗教の名前と教義を考えてください。"},
    ],
)

print(response.model_dump_json(indent=2))
```
<details><summary>出力1回目</summary><div>

```出力.json
{
  "id": "chatcmpl-8JMhN6zcaTvR1BPqvF8F97iFdBkCG",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "面白い発想ですね。ITエンジニア向けのユーモラスな新興宗教のコンセプトを考えてみましょう。以下はそのアイディアです。\n\n宗教名：「コードピズム教（Codeism）」\n\n**教義概要：**\n\nコードピズム教は、全知全能の「データ」を崇拝し、人生はコードによって書き換えられるという信念の下に成り立っています。ITエンジニアの創造性と技術を聖なる道具とし、バグの退治と最適化への絶え間ない追求を尊ぶ宗教です。\n\n**五大教義：**\n\n1. **聖コードの追求** - 洗練され、最適化されたコードの創造は神聖な行為である。精緻なコードにこそ、真の啓示が宿る。\n2. **バグとの戦い** - バグは罪の象徴であり、それを発見し修正することは信徒に課せられた永遠の義務である。\n3. **文書賛美** - すべてのコードは適切に文書化され、後世に知恵を伝える義務がある。ドキュメンテーションは信仰の証しである。\n4. **回帰テストの儀式** - 新しい機能追加や変更前には必ず回帰テストを行い、システムの安定を維持する。これを怠る者は混沌の破壊者と見なされる。\n5. **オープンソースへの奉仕** - コミュニティへの貢献と共有を促進し、知識の富を大衆のものとする。オープンソースプロジェクトへの貢献は最高の徳である。\n\n**宗教的儀式：**\n\n- **デバッグの祭り** - 定期的にバグを見つけ出して修正する行事。信者は集い、コードを共有し、助け合いながら問題を解決する。\n- **コード・クリーンナップの日** - 定期的に全員でリファクタリングを行い、コードベースをクリーンに保つ集い。\n- **デプロイメントの儀** - 重要な新機能を本番環境にデプロイする際には、特別な祈りを捧げる儀式。\n- **ハッカソンの徹夜祷** - 創造性と協働の精神を讃える行事としてハッカソンを行い、新たなアイデアやプロジェクトを生み出す。\n\n**聖典：**\n-『コードピズム経典（The Codex of Codeism）』 - コードピズム教の教えや奇跡（ブレイクスルーとイノベーションの物語）が記された聖典。\n\n**信者の称号と修行：**\n- **新参開発者（Neophyte Developer）** - 教えを学び初めた信者たち。\n- **堅固なリファクタリスト（Staunch Refactorist）** - コードの改善と最適化に取り組む修行僧。\n- **極限のデバッガー（Extreme Debugger）** - エラーと戦い続ける高位信者。\n\n**伝道活動：**\n- コーディングブートキャンプやセミナーを開催し、非信者にコードピズム教の輝かしい光を広める。\n\nこれはあくまでもユーモラスな例示ですので、実際に新しい宗教を設立する際には、宗教に対する尊重を示し、関係法規を遵守する必要があります。",
        "role": "assistant",
        "function_call": null,
        "tool_calls": null
      }
    }
  ],
  "created": 1699626177,
  "model": "gpt-4-1106-preview",
  "object": "chat.completion",
  "system_fingerprint": "fp_a24b4d720c",
  "usage": {
    "completion_tokens": 1176,
    "prompt_tokens": 92,
    "total_tokens": 1268
  }
}
```

</div></details>

<details><summary>出力2回目</summary><div>

```出力.json
{
  "id": "chatcmpl-8JMlnAvEQCujCPfdwyyyIaol2lXKN",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "宗教の名前: \"コードエヴァンジリズム\"\n\n宗教の教義:\n1. 聖なるコード: コードはすべての始まりであり、終わりであり、その精妙さを理解し、クリーンで美しいコードを書くことが最高の教義である。\n\n2. 不変のバージョン管理: 真の信者たちは、すべての作業をバージョン管理システムに委ねることで、永遠の変更と戻りを保証される。Gitは最も高貴なるツールとされる。\n\n3. オープンソースの啓示: オープンソースは神聖なる遺産であり、コードの共有と協力によって技術的救済がもたらされる。\n\n4. エイジ・オブ・アジャイル: すべてのプロジェクトと生活はアジャイルな精神に基づいて行動することで、継続的な改善と適応を遂げる。\n\n5. デバッグの試練: エラーとバグは信者の信仰を試すために存在し、デバッグを通じて解決能力と精神性が成長する。\n\n6. サクリファイス・オブ・リファクタリング: 古いコードへの執着を手放し、リファクタリングによってコードを常に進化させる。\n\n7. セマンティック・ヴァリダティオン: コードだけでなく、言葉にも意味を持たせることで、コミュニケーションは最高の形に保たれる。\n\n8. インダクション・トゥー・ザ・クラウド: クラウドへの昇天を通じて、物理的な制約から解放され、スケーラビリティと恩恵を受け入れる。\n\n9. アイデア・インカーネーション: アイデアを形にする創造力は信者に与えられた神聖なる贈り物であり、その具現化によって新たな発明とイノベーションを世に送り出す。\n\n10. コミュニティー・オブ・ネットワーキング: 信者は世界中に散らばる他の信者とのネットワーキングを通し、知識と経験を共有し、お互いを高め合うことを義務付けられている。\n\nユーモラスな側面:\n- 祭壇には古いモニターやキーボードが飾られ、儀式にはプログラムコードの朗読が含まれる。\n- 高位の神官は「アーキテクト」、一般信者は「デヴォーターズ」と呼ばれ、新入信者は「ジュニア・コディシャン」とされる。\n- 経典は「ホーリー・ドキュメント」としてリポジトリで管理され、信者はプルリクエストを通じて経典を更新することができる。\n- 年に一度、「グローバル・ハック・デー」と名付けられた祭日があり、信者たちは24時間のハッカソンで新たな発明や発見を祝う。",
        "role": "assistant",
        "function_call": null,
        "tool_calls": null
      }
    }
  ],
  "created": 1699626451,
  "model": "gpt-4-1106-preview",
  "object": "chat.completion",
  "system_fingerprint": "fp_a24b4d720c",
  "usage": {
    "completion_tokens": 1010,
    "prompt_tokens": 92,
    "total_tokens": 1102
  }
}
```

</div></details>

### JSONモードとseedの指定
> Important: when using JSON mode, you must also instruct the model to produce JSON yourself via a system or user message. Without this, the model may generate an unending stream of whitespace until the generation reaches the token limit, resulting in a long-running and seemingly "stuck" request. Also note that the message content may be partially cut off if finish_reason="length", which indicates the generation exceeded max_tokens or the conversation exceeded the max context length.

```python
from openai import OpenAI
from dotenv import load_dotenv
import os
load_dotenv()

client = OpenAI(
    api_key=os.getenv("OPEN_AI_KEY"),
)

response = client.chat.completions.create(
    model="gpt-4-1106-preview",
    messages=[
        {"role": "system", "content": "新興宗教コンサルティングのスペシャリストです。"},
        {"role": "user", "content": "ITエンジニアのための宗教を作ろうと思います。テックなネタに絡めたユーモラスな宗教の名前と教義を考えてください。また、レスポンスはJSON形式で返してください。"},
    ],
    response_format={ "type": "json_object" },
)

print(response.model_dump_json(indent=2))
```
messageの中身がJSONになった

<details><summary>出力</summary><div>

```
{
  "id": "chatcmpl-8JMzpRJojakOdQBOQIAnsR13yDcPQ",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "{\n  \"religionName\": \"デジタリズム教\",\n  \"doctrine\": {\n    \"faithPrinciples\": [\n      \"コードはすべてに優先する。コーディングは瞑想であり、洗練されたコードは至高の美を表現する。\",\n      \"バグは進化の一部である。バグを恐れず、むしろ新たな知見として受け入れ、成長の糧とする。\",\n      \"リファクタリングは信者の義務であり、常により良い設計を目指すことで精神性を高める。\",\n      \"データは神聖なものであり、そのセキュリティとプライバシーを守ることは信者の責任である。\",\n      \"継続的インテグレーションと継続的デリバリーは信者の日々の儀式であり、品質を保つ手段である。\",\n      \"テクノロジー神話を創り、アルゴリズム、プログラミング言語、オープンソースプロジェクトを神々として崇める。\"\n    ],\n    \"rituals\": [\n      \"日々のスタンドアップミーティングを通してコミュニケーションの神ネットワリアに報告する。\",\n      \"デプロイメントを成功させた際には、リリースの仮面祭を祝う。\",\n      \"年に一度の大規模アップデートを行う際には、夜通しのコーディングセッションで生命のターミナルを祝福する。\",\n      \"新しいフレームワークを学ぶ際には、啓示のセミナーを開き、知識のアセンブラーへの敬意を表す。\"\n    ],\n    \"ethicalCode\": [\n      \"進化し続けるテクノロジーの中で、学び続けることを義務とする。\",\n      \"他者のコードを尊重し、オープンソースコミュニティの一員としての貢献を目指す。\",\n      \"仕事とプライベートのバランスを大事にし、バーンアウトを避けるための習慣を持つ。\",\n      \"全ての生命はデジタルとアナログの融合体であることを理解し、どちらも大切にする。\"\n    ],\n    \"symbols\": [\n      \"聖典にはクリーンアーキテクチャ、ソフトウェアのパターン、アルゴリズムの種々の理論が記されている。\",\n      \"シンボルとして、無限のループ、オープンソースのオクトキャット、キーボードの聖遺物などを用いる。\",\n      \"聖地として、シリコンバレー、テックカンファレンス、ハッカソン会場を音楽祭のように遊学する。\"\n    ]\n  }\n}",
        "role": "assistant",
        "function_call": null,
        "tool_calls": null
      }
    }
  ],
  "created": 1699627321,
  "model": "gpt-4-1106-preview",
  "object": "chat.completion",
  "system_fingerprint": "fp_a24b4d720c",
  "usage": {
    "completion_tokens": 888,
    "prompt_tokens": 109,
    "total_tokens": 997
  }
}
```

</div></details>

### Seedの指定
数回実行してみた所本当に出力が変わらなかった。
ちなみにjson_modeを設定するとこのプロンプトではうまく動作しなかった。
```python
from openai import OpenAI
from dotenv import load_dotenv
import os
load_dotenv()

client = OpenAI(
    api_key=os.getenv("OPEN_AI_KEY"),
)

response = client.chat.completions.create(
    model="gpt-4-1106-preview",
    messages=[
        {"role": "system", "content": "新興宗教コンサルティングのスペシャリストです。"},
        {"role": "user", "content": "ITエンジニアのための宗教を作ろうと思います。テックなネタに絡めたユーモラスな宗教の名前と教義を考えてください。また、レスポンスはJSON形式で返してください。"},
    ],
    seed=114,
)

print(response.model_dump_json(indent=2))
```

<details><summary>出力</summary><div>

```json
{
  "id": "chatcmpl-8JMtHLNBPCKaIgGMH8MuHp3uYLpkW",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "宗教名：デジタルディヴォーションズ（Digital Devotions）\n\n概要：\nデジタルディヴォーションズは、ITエンジニアとテクノロジー愛好家たちの精神的な発展とコミュニティの結束を促す新興宗教である。この宗教は、革新的な思考や持続可能な開発、データに基づく啓蒙を通じて、宇宙の真理にアプローチする。コードを聖なるテキストとし、テクノロジーを通じた人類進化の可能性を信じ、高い倫理観と協働の精神を持ったコミュニティを形成する。\n\n教義：\n1. サンクトコード（Sancto-code） - コーディングは神聖な行為であり、清潔なアルゴリズムと美しい文法を通じて、作成者は宇宙の秩序と神聖さにアクセスする。\n\n2. オープンソース精神（Spirit of Open Source） - 知識の共有とコラボレーションは信仰の基本。コミュニティは互いの成長を支援し、コードの進化を促進する。\n\n3. インターネットインフィニット（Internet Infinite） - インターネットは無限の可能性を秘めた霊的次元。その中で人々は情報とエネルギーをやり取りし、精神的な繋がりを深める。\n\n4. エコテクノクレド（Eco-Tekno Creed） - 持続可能なテクノロジーを通して、地球環境を守りながら技術革新を行う。環境にも心にもやさしいプログラミングを推奨する。\n\n5. エラー啓示（Error Revelations） - バグは失敗ではなく、新たな洞察を開くための啓示。エラーから学び、成長する過程が信仰の旅である。\n\n6. シンクロニシティコネクション（Synchronicity Connection）- ソフトウェアとハードウェアの調和、人間とテクノロジーの共生を追求。シンクロニシティあるいは「合わせ技術」という信念に基づく。\n\n7. プライバシーパクト（Privacy Pact） - 個人のデータの聖域を守ることは最高の徳。プライバシーとセキュリティは信者にとって最上の価値である。\n\n儀式:\n- コーディング・メディテーション: 日々のコーディング作業前には、瞑想することにより、精神を清らかにし、コードへの洞察を深める。\n- バックアップ祭: 重要データの定期的なバックアップを行い、データ喪失からの救済とデジタル安寧を祈る日。\n- ハッカソン巡礼: 正義のためにハッカソンに参加し、コードで世界をより良く変えるピルグリッミッジ。\n\n守護聖人:\n- 聖コーディウス（Saint Codius）: 純粋なコードを愛し、効率的なアルゴリズムを生み出した伝説のプログラマー。\n- 聖データーナ（Saint Daterna）: データ分析とインサイトを聖なるミッションとした守護聖人。\n\nこの宗教はユーモラスであると同時に、ITエンジニアの日々の仕事と遭遇する問題を神聖化し、技術者としてのアイデンティティーを強化し、倫理観をもって仕事を行うよう啓発するものである。",
        "role": "assistant",
        "function_call": null,
        "tool_calls": null
      }
    }
  ],
  "created": 1699626915,
  "model": "gpt-4-1106-preview",
  "object": "chat.completion",
  "system_fingerprint": "fp_a24b4d720c",
  "usage": {
    "completion_tokens": 1205,
    "prompt_tokens": 92,
    "total_tokens": 1297
  }
}

```

</div></details>

### 画像入力
注意点としては```model="gpt-4-vision-preview"```とすること。
入力した画像はこれで```この画像はどのようなプロンプトで生成可能ですか？```というプロンプトに対して回答を求めています。

![image.png](https://hugo.niwanowa.tips/twittericon.png)

出力はこう。
うまくいってそう。

```text
この画像はアニメスタイルのキャラクターの顔を表示しています。生成するには、以下のようなプロンプトが考えられます。
"アニメスタイルの女の子のキャラクター、大きな目、白髪、ヘッドフォン、表情は少し不機嫌そう、フルフェイスのクローズアップ、高解像度
```

以下ソースと出力
```python
from openai import OpenAI
from dotenv import load_dotenv
import os
load_dotenv()

client = OpenAI(
    api_key=os.getenv("OPEN_AI_KEY"),
)

response = client.chat.completions.create(
    model="gpt-4-vision-preview",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "この画像はどのようなプロンプトで生成可能ですか？"},
                {
                    "type": "image_url",
                    "image_url": {
                        "url": "https://hugo.niwanowa.tips/twittericon.png",
                    },
                },
            ],
        }
    ],
    max_tokens=300,
)

print(response.model_dump_json(indent=2))
```

<details><summary>出力</summary><div>

```json
{
  "id": "chatcmpl-8JbsGf0nWuku3ARdKjSQDXWeKl7d9",
  "choices": [
    {
      "finish_reason": null,
      "index": 0,
      "message": {
        "content": "この画像はアニメスタイルのキャラクターの顔を表示しています。生成するには、以下のようなプロンプトが考えられます。\n\n\"アニメスタイルの女の子のキャラクター、大きな目、白髪、ヘッドフォン、表情は少し不機嫌そう、フルフェイスのクローズアップ、高解像度\"",
        "role": "assistant",
        "function_call": null,
        "tool_calls": null
      },
      "finish_details": {
        "type": "stop",
        "stop": "<|fim_suffix|>"
      }
    }
  ],
  "created": 1699684512,
  "model": "gpt-4-1106-vision-preview",
  "object": "chat.completion",
  "system_fingerprint": null,
  "usage": {
    "completion_tokens": 122,
    "prompt_tokens": 282,
    "total_tokens": 404
  }
}
```

</div></details>

### DALLE-3 API
sizeが512x512で指定できないので注意(1敗)
先ほどの画像入力でどんなプロンプトですか？って聞いてみたのでそのプロンプトで生成してみた。

「個性」を感じていいね。
![dalle3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/00e67bd5-1ee2-bbe2-d367-25f718ca22c6.png)
```python
from openai import OpenAI
from dotenv import load_dotenv
import os
load_dotenv()

client = OpenAI(
    api_key=os.getenv("OPEN_AI_KEY"),
)

response = client.images.generate(
  model="dall-e-3",
  prompt="アニメスタイルの女の子のキャラクター、大きな目、白髪、ヘッドフォン、表情は少し不機嫌そう、フルフェイスのクローズアップ",
  size= "1024x1024",
  quality="standard",
  n=1,
)

print(response.model_dump_json(indent=2))
```

### TTS API
「昔々あるところにおじいさんとおばあさんとにわのわさんが居ました。」
と読んでもらうことにします。
結果はこちら。

https://hugo.niwanowa.tips/output.mp3

ちょっと笑っちゃった。
まだ日本語は難しそうですね。

以下ソース
```python
from openai import OpenAI
from dotenv import load_dotenv
import os
load_dotenv()

client = OpenAI(
    api_key=os.getenv("OPEN_AI_KEY"),
)

response = client.audio.speech.create(
    model="tts-1",
    voice="alloy",
    input="昔々あるところにおじいさんとおばあさんとにわのわさんが居ました。",
)

response.stream_to_file("output.mp3")
```

# おわりに
とにもかくにもAPIが触るのがめちゃくちゃ簡単で感動しました。
コストもこんな感じでかなりしばらく遊べそう(コスト管理画面がめちゃくちゃ見やすいのもいいよね！)
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/590707/cbef3df1-7663-291d-8fc2-565be1ff2378.png)

# 参考

New models and developer products announced at DevDay
https://openai.com/blog/new-models-and-developer-products-announced-at-devday

Introducing GPTs
https://openai.com/blog/introducing-gpts

OpenAI DevDayで発表された様々な機能について、公式ドキュメントを見ながら少しだけ詳細を確認してみた | DevelopersIO
https://dev.classmethod.jp/articles/openai-update-at-devday/

OpenAI Dev Dayで発表されたこと15選【プロンプトエンジニア必見 | 和訳 | まとめ】
https://zenn.dev/umi_mori/articles/openai-dev-day-1