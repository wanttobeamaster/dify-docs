# ツールプラグイン

Tool（ツール）プラグインは、チャットフロー、ワークフロー、エージェントといったアプリタイプから参照できる外部ツールであり、Difyアプリの機能を拡張するために使用されます。例えば、アプリにオンライン検索機能や画像生成機能を追加するといったことが可能です。ツールプラグインは、包括的なツールセットとAPI実装機能を提供します。

<figure><img src="https://assets-docs.dify.ai/2024/12/7e7bcf1f9e3acf72c6917ea9de4e4613.png" alt=""><figcaption></figcaption></figure>

本篇では、「ツールプラグイン」とは、ツールプロバイダーファイル、機能コード、およびその他の関連コンポーネントを含む、一揃いのプロジェクトを指します。ツールプロバイダーは複数のツール（これは、単一のツールが提供する追加機能と解釈できます）を含むことがあり、以下の構造で構成されます。

```
- ツールプロバイダー
    - Tool A
    - Tool B
```

![ツールの構造](https://assets-docs.dify.ai/2025/02/60c4c86a317d865133aa460592eac079.png)

この記事では、`GoogleSearch`を例に、ツールプラグインを迅速に開発する方法をご紹介します。

### 事前準備

* Dify プラグインの Scaffolding（スキャフォールディング）ツール
* Python 環境 (バージョン 3.12 以上)

プラグイン開発用のScaffoldingツールの準備方法については、[初期化開発ツール](initialize-development-tools.md)をご参照ください。

### 新規プロジェクトの作成

Scaffoldingのコマンドラインツールを実行し、新しいDifyプラグインプロジェクトを作成します。

```bash
./dify-plugin-darwin-arm64 plugin init
```

バイナリファイルの名前を `dify` に変更し、`/usr/local/bin` ディレクトリにコピーした場合は、次のコマンドを実行して新しいプラグインプロジェクトを作成できます。

```bash
dify plugin init
```

> 以下、コマンドラインツール `dify` を使用します。もし問題が発生した場合は、`dify` コマンドを、お使いのツールの実行パスに置き換えてください。

### プラグインの種類とテンプレートの選択

プラグインには、ツール、モデル、そしてエクステンションの3種類があります。SDK内のすべてのテンプレートには、完全なコードプロジェクトが付属しています。以下の部分では、**ツールプラグイン**テンプレートを例として使用します。

> プラグイン開発に精通されている方は、各種プラグインの実装にあたり、[スキーマ仕様](../../schema-definition/README.md)をご参照ください。

![プラグインタイプ: ツール](https://assets-docs.dify.ai/2024/12/dd3c0f9a66454e15868eabced7b74fd6.png)

#### プラグイン権限の設定

プラグインを正常に接続するには、Difyプラットフォームの権限を読み取る必要があります。このサンプルツールプラグインには、以下の権限を付与する必要があります。

* ツール
* アプリ
* 永続的ストレージジストレージの有効化、デフォルトサイズのストレージの割り当て
* エンドポイントの登録許可

> ターミナルで方向キーを使って権限を選択し、"タグ"のブタンで権限を付与してください。

すべての権限項目にチェックを入れたら、Enterキーを押してプラグインの作成を完了させてください。システムが自動的にプラグインプロジェクトのコードを生成します。

![プラグインの権限](https://assets-docs.dify.ai/2024/12/9cf92c2e74dce55e6e9e331d031e5a9f.png)

### 開発ツールプラグイン

#### 1. ツールプロバイダーの yaml ファイルを作成する

ツールプロバイダーファイルは、ツールプラグインの基本的な設定ファイルとして機能し、ツールが利用するために必要な認証情報を提供します。このセクションでは、この `yaml` ファイルの記述方法について説明します。

`/provider` ディレクトリに移動し、その中にある yaml ファイルの名前を `google.yaml` に変更します。この `yaml` ファイルには、プロバイダー名、アイコン、作成者など、ツールプロバイダーに関する情報が含まれます。これらの情報は、プラグインのインストール時に表示されます。

**サンプルコード**

```yaml
identity: # ツールプロバイダーの基本情報
  author: Your-name # 作成者
  name: google # 名前。一意である必要があり、他のプロバイダーと重複できません
  label: # ラベル。フロントエンドでの表示に使用
    en_US: Google # 英語ラベル
    zh_Hans: Google # 中国語ラベル
    ja_JP: Google # 日本語ラベル
    pt_BR: Google # プルトガル語ラベル
  description: # 説明。フロントエンドでの表示に使用
    en_US: Google # 英語説明
    zh_Hans: Google # 中国語説明
    ja_JP: : Google # 日本語説明
    pt_BR: : Google # プルトガル語説明
  icon: icon.svg # アイコン。_assets フォルダに配置する必要があります
  tags: # タグ。フロントエンドでの表示に使用
    - search
```

* `identity` には、作成者、名前、ラベル、説明、アイコンなど、ツールプロバイダーの基本的な情報が含まれます。
  * アイコンは添付ファイルとして扱い、プロジェクトのルートディレクトリにある `_assets` フォルダに配置する必要があります。
  * タグは、ユーザーがプラグインをカテゴリ別にすばやく検索するのに役立ちます。現在サポートされているすべてのタグは以下のとおりです。
  * ```python
    class ToolLabelEnum(Enum):
      SEARCH = 'search'
      IMAGE = 'image'
      VIDEOS = 'videos'
      WEATHER = 'weather'
      FINANCE = 'finance'
      DESIGN = 'design'
      TRAVEL = 'travel'
      SOCIAL = 'social'
      NEWS = 'news'
      MEDICAL = 'medical'
      PRODUCTIVITY = 'productivity'
      EDUCATION = 'education'
      BUSINESS = 'business'
      ENTERTAINMENT = 'entertainment'
      UTILITIES = 'utilities'
      OTHER = 'other'
    ```

ファイルパスが `/tools` ディレクトリにあることを確認してください。完全なパスは次のようになります。

```yaml
plugins:
  tools:
    - "google.yaml"
```

ここで、`google.yaml` ファイルのパスは、プラグインプロジェクト内の絶対パスで指定する必要があります。

* **サードパーティサービスの認証情報を設定する**

開発を容易にするため、サードパーティサービスである `SerpApi` が提供する Google Search API を利用することにしました。`SerpApi` を使用するには API キーが必要となるため、`yaml` ファイルに `credentials_for_provider` フィールドを追加する必要があります。

完全なコードは次のとおりです。

```yaml
identity:
  author: Dify
  name: google
  label:
    en_US: Google
    zh_Hans: Google
    ja_JP: Google
    pt_BR: Google
  description:
    en_US: Google
    zh_Hans: GoogleSearch
    ja_JP: Google
    pt_BR: Google
  icon: icon.svg
  tags:
    - search
credentials_for_provider: # credentials_for_provider フィールドを追加
  serpapi_api_key:
    type: secret-input
    required: true
    label:
      en_US: SerpApi API key
      zh_Hans: SerpApi API key
      ja_JP: SerpApi API key
      pt_BR: chave de API SerpApi
    placeholder:
      en_US: Please input your SerpApi API key
      zh_Hans: 请输入你的 SerpApi API key
      ja_JP: SerpApi API keyを入力してください
      pt_BR: Por favor, insira sua chave de API SerpApi
    help:
      en_US: Get your SerpApi API key from SerpApi
      zh_Hans: 从 SerpApi 获取您的 SerpApi API key
      ja_JP: SerpApiからSerpApi APIキーを取得する
      pt_BR: Obtenha sua chave de API SerpApi da SerpApi
    url: https://serpapi.com/manage-api-key
tools:
  - tools/google_search.yaml
extra:
  python:
    source: google.py
```

* `credentials_for_provider` の子構造は、[ProviderConfig](../../schema-definition/general-specifications.md#providerconfig) の仕様に準拠する必要があります。
* このプロバイダーに含まれるツールを指定する必要があります。この例では、`tools/google_search.yaml` ファイルが 1 つだけ含まれています。
* プロバイダーは、基本情報を定義するだけでなく、コードによるロジックの実装も必要です。そのため、実装ロジックを指定する必要があります。この例では、機能のコードファイルは `google.py` に配置されていますが、ここではまだ実装せず、先に `google_search` のコードを作成します。これは、`google_search` の機能が `google.py` の実装に依存するためです。

#### 2. ツールの YAML ファイルへの入力

1つのツールベンダーが複数のツールを提供することがあります。各ツールは、その基本的な情報、パラメータ、出力などを記述した `yaml` ファイルによって定義される必要があります。

ここでは、`GoogleSearch` ツールを例にとり、`/tools` フォルダに新しい `google_search.yaml` ファイルを作成する手順を見ていきましょう。

```yaml
identity:
  name: google_search
  author: Dify
  label:
    en_US: GoogleSearch
    zh_Hans: 谷歌搜索
    ja_JP: Google検索
    pt_BR: Pesquisa Google
description:
  human:
    en_US: A tool for performing a Google SERP search and extracting snippets and webpages.Input should be a search query.
    zh_Hans: 一个用于执行 Google SERP 搜索并提取片段和网页的工具。输入应该是一个搜索查询。
    ja_JP: Google SERP 検索を実行し、スニペットと Web ページを抽出するためのツール。入力は検索クエリである必要があります。
    pt_BR: Uma ferramenta para realizar pesquisas no Google SERP e extrair snippets e páginas da web. A entrada deve ser uma consulta de pesquisa.
  llm: A tool for performing a Google SERP search and extracting snippets and webpages.Input should be a search query.
parameters:
  - name: query
    type: string
    required: true
    label:
      en_US: Query string
      zh_Hans: 查询字符串
      ja_JP: クエリ文字列
      pt_BR: Cadeia de consulta
    human_description:
      en_US: used for searching
      zh_Hans: 用于搜索网页内容
      ja_JP: ネットの検索に使用する
      pt_BR: usado para pesquisar
    llm_description: key words for searching
    form: llm
extra:
  python:
    source: tools/google_search.py
```

*   `identity`: ツールの名前、作成者、ラベル、説明といった基本的な情報が含まれます。
*   `parameters`: パラメータに関する設定項目です。各項目の詳細は以下の通りです。
    *   `name` (必須): パラメータ名。一意である必要があり、他のパラメータ名との重複は許容されません。
    *   `type` (必須): パラメータの型。`string` (文字列)、`number` (数値)、`boolean` (真偽値)、`select` (選択式ドロップダウン)、`secret-input` (暗号化入力フィールド)、`file` (ファイル)、`files` (ファイルセット)、`model-selector` (モデル選択)、`app-selector` (アプリケーション選択) の9種類がサポートされています。機密性の高い情報を取り扱う場合は、必ず `secret-input` 型を使用してください。
    *   `label` (必須): パラメータのラベル。フロントエンドに表示される際に用いられます。
    *   `form` (必須): フォームの種類。`llm` と `form` の2種類がサポートされています。
        *   Agentアプリケーションにおいて、`llm` はパラメータがLLMによって推論されることを意味し、`form` はツールを使用する前にユーザーが設定できるパラメータを意味します。
        *   ワークフローアプリケーションにおいては、`llm` と `form` の両方のパラメータに対してフロントエンドでの入力が求められます。ただし、`llm` パラメータはツールノードの入力変数として機能します。
    *   `required`: 必須項目であるかどうかを示すフラグです。
        *   `llm` モードの場合、`required` が `true` に設定されたパラメータは、Agentが推論によって値を決定する必要があります。
        *   `form` モードの場合、`required` が `true` に設定されたパラメータは、ユーザーが会話を開始する前にフロントエンドで値を入力する必要があります。
    *   `options`: パラメータの選択肢。
        *   `llm` モードでは、Dify はすべての選択肢をLLMに渡し、LLM はこれらの選択肢に基づいて推論を行います。
        *   `form` モードでは、`type` が `select` に設定されている場合、フロントエンドにこれらの選択肢が表示されます。
    *   `default`: デフォルト値。
    *   `min`: 最小値。パラメータの型が `number` の場合に設定可能です。
    *   `max`: 最大値。パラメータの型が `number` の場合に設定可能です。
    *   `human_description`: フロントエンドに表示されるパラメータの説明文です。多言語に対応しています。
    *   `placeholder`: 入力フィールドに表示されるプレースホルダーテキストです。`form` 形式で、パラメータの型が `string`、`number`、`secret-input` の場合に設定できます。多言語に対応しています。
    *   `llm_description`: LLMに渡されるパラメータの説明文です。LLMがパラメータの内容をより深く理解できるよう、できる限り詳細な情報を提供してください。

#### 3. ツールコードの準備

ツールの設定情報を入力したら、ツールの機能コードを記述し、ツールのロジックを実装します。 `/tools` ディレクトリに `google_search.py` を作成し、以下の内容を記述します。

```python
from collections.abc import Generator
from typing import Any

import requests

from dify_plugin import Tool
from dify_plugin.entities.tool import ToolInvokeMessage

SERP_API_URL = "https://serpapi.com/search"

class GoogleSearchTool(Tool):
    def _parse_response(self, response: dict) -> dict:
        result = {}
        if "knowledge_graph" in response:
            result["title"] = response["knowledge_graph"].get("title", "")
            result["description"] = response["knowledge_graph"].get("description", "")
        if "organic_results" in response:
            result["organic_results"] = [
                {
                    "title": item.get("title", ""),
                    "link": item.get("link", ""),
                    "snippet": item.get("snippet", ""),
                }
                for item in response["organic_results"]
            ]
        return result

    def _invoke(self, tool_parameters: dict[str, Any]) -> Generator[ToolInvokeMessage]:
        params = {
            "api_key": self.runtime.credentials["serpapi_api_key"],
            "q": tool_parameters["query"],
            "engine": "google",
            "google_domain": "google.com",
            "gl": "us",
            "hl": "en",
        }

        response = requests.get(url=SERP_API_URL, params=params, timeout=5)
        response.raise_for_status()
        valuable_res = self._parse_response(response.json())
        
        yield self.create_json_message(valuable_res)
```

この例では、`serpapi` にリクエストを送信し、`self.create_json_message` を使用して `json` 形式のデータを返しています。その他の戻り値の型については、[ツールインターフェースドキュメント](../../schema-definition/tool.md)を参照してください。

#### 4. ツールプロバイダーコードの作成

最後に、プロバイダーのコードを実装します。これは、プロバイダーの認証情報を検証するために使用されます。認証情報の検証に失敗すると、`ToolProviderCredentialValidationError` 例外が発生します。検証に成功すると、`google_search` ツールサービスが正常にリクエストされます。

`/provider` ディレクトリに `google.py` ファイルを作成し、以下のコードを記述します。

```python
from typing import Any

from dify_plugin import ToolProvider
from dify_plugin.errors.tool import ToolProviderCredentialValidationError
from tools.google_search import GoogleSearchTool

class GoogleProvider(ToolProvider):
    def _validate_credentials(self, credentials: dict[str, Any]) -> None:
        try:
            for _ in GoogleSearchTool.from_credentials(credentials).invoke(
                tool_parameters={"query": "test", "result_type": "link"},
            ):
                pass
        except Exception as e:
            raise ToolProviderCredentialValidationError(str(e))
```

### プラグインのデバッグ

プラグインが正常に動作するかテストします。Dify はリモートデバッグをサポートしています。「プラグイン管理」ページで、デバッグキーとリモートサーバーアドレスを取得してください。

<figure><img src="https://assets-docs.dify.ai/2024/12/053415ef127f1f4d6dd85dd3ae79626a.png" alt=""><figcaption></figcaption></figure>

プラグインプロジェクトに戻り、`.env.example` ファイルをコピーして `.env` にリネームし、取得したリモートサーバーアドレスとデバッグキーを入力します。

`.env` ファイル

```bash
INSTALL_METHOD=remote
REMOTE_INSTALL_HOST=remote
REMOTE_INSTALL_PORT=5003
REMOTE_INSTALL_KEY=****-****-****-****-****
```

`python -m main` コマンドを実行してプラグインを起動します。プラグインページで、プラグインがワークスペースにインストールされていることを確認できます。これで、他のチームメンバーもこのプラグインを使用できます。

<figure><img src="https://assets-docs.dify.ai/2024/11/0fe19a8386b1234755395018bc2e0e35.png" alt=""><figcaption></figcaption></figure>

### プラグインのパッケージ化

プラグインが正常に動作することを確認したら、以下のコマンドラインツールを使用してプラグインをパッケージ化し、名前を付けます。実行後、現在のフォルダに `google.difypkg` ファイルが作成されます。これがプラグインの最終的なパッケージです。

```bash
# Replace ./google with your actual plugin project path.

dify plugin package ./google
```

おめでとうございます。これで、ツールタイプのプラグインの開発、デバッグ、パッケージ化の全プロセスが完了しました。

### プラグインの公開

[Dify Plugins コードリポジトリ](https://github.com/langgenius/dify-plugins)にアップロードして、プラグインを公開しましょう。アップロードする前に、プラグインが[プラグイン公開仕様](https://docs.dify.ai/ja-jp/plugins/publish-plugins/publish-to-dify-marketplace)に準拠していることを確認してください。レビューに合格すると、コードはメインブランチにマージされ、[Dify Marketplace](https://marketplace.dify.ai/) に自動的に公開されます。

#### さらに詳しく

**クイックスタート:**

* [拡張タイププラグインの開発](extension-plugin.md)
* [モデルタイププラグインの開発](model-plugin/)
* [バンドルタイププラグイン：複数のプラグインをパッケージ化](bundle.md)

**プラグインインターフェースドキュメント:**

* [マニフェスト](../../schema-definition/manifest.md) 構造
* [エンドポイント](../../schema-definition/endpoint.md) 詳細定義
* [Difyサービスの逆呼び出し](../../schema-definition/reverse-invocation-of-the-dify-service/)
* [ツール](../../schema-definition/tool.md)
* [モデル](../../schema-definition/model/)
* [拡張エージェント戦略](../../schema-definition/agent.md)