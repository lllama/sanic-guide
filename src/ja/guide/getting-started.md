# スタートアップ

最初に、Python 3.7以降が実行されていることを確認します。 現在、はPythonバージョン3.7、3.8、および3.9で動作することがわかっています。

## インストール

```bash
pip install sanic
```

## Hello, worldアプリケーション

---:1

多くのデコレーター・ベースのフレームワークのいずれかを使用したことがある人であれば、このフレームワークはおそらく皆さんにとって馴染みのあるものです。

::: tip 
もしあなたがFlaskや他のフレームワークから来ているのであれば、いくつかの重要な指摘があります。 Sanicはパフォーマンス、柔軟性、使いやすさを目指しています。 これらの指針は、APIとその動作に目に見える影響を与えます。
:::



:--:1

```python
from sanic import Sanic
from sanic.response import text

app = Sanic("MyHelloWorldApp")

@app.get("/")
async def hello_world(request):
    return text("Hello, world.")
```

:---

### ご注意ください

- すべてのリクエストハンドラは、sync(`def hello_world`)またはasync(`async def hello_world`)のいずれかになります。 明確な理由がない限り、常に`async'を使ってください。
- `request`オブジェクトは、常にハンドラの最初の引数です。 他のフレームワークは、インポートされるコンテキスト変数でこれを渡します。 `async`の世界では、これはあまりうまく動作しないでしょうし、それについて明示的に説明することは (よりクリーンでよりパフォーマンス的であることは言うまでもありませんが) はるかに簡単です。
- レスポンスタイプを**必ず**使用してください。 他の多くのフレームワークでは、`return "Hello, world."`または`return {"foo":"bar"}`のようにreturnするのを許可します。 しかし、この暗黙の呼び出しを行うには、チェーン内のどこかで、意味を判断するために貴重な時間を費やす必要があります。 この容易さを犠牲にして、Sanicは明示的なコールを要求することにしました。

### 実行

---:1 上記のファイルを`server.py`として保存しましょう。 そして、起動してみます。 :--:1
```bash
sanic server.app
```
:---

::: tip これは**もう一つの**重要な他との違いです。 他のフレームワークには、組み込みの開発サーバーが付属しており、開発_専用_であることを明示的に示しています。 その逆はサニックに当てはまります。

**パッケージ化されたサーバーは、本番環境での実行に対応しています。 ** :::

## Sanicエクステンション

Sanicは、意図的にクリーンで偏見のない機能リストを目指しています。 プロジェクトは、特定の方法でアプリケーションを構築することを要求したくないし、特定の開発パターンを処方することを避けようとしている。 コアリポジトリの要件を満たさない追加機能を追加するために、コミュニティによって構築・維持されているサードパーティプラグインが多数あります。

しかし、**API開発者を助けるために**、Sanic組織は[Sanic Extensions](../plugins/sanic-ext/getting-started.md) という公式プラグインを維持しており、以下を含むあらゆる種類のグッズを提供しています。

- Redoc および/または Swagger による **OpenAPI** ドキュメンテーション
- CORS**保護**
- ルートハンドラへの**依存性注入**
- リクエストのクエリ引数とボディ入力の**検証**。
- `HEAD`、`OPTIONS`、`TRACE`のエンドポイントを自動作成する。
- 定義済み、エンドポイント固有のレスポンスシリアライザー

設定方法としては、Sanicと一緒にインストールするのが望ましいですが、パッケージ単体でインストールすることも可能です。

---:1
```
$ pip install sanic[ext]
```
:--:1
```
$ pip install sanic sanic-ext
```
:---

v21.12から、Sanicが同じ環境であれば、Sanic Extensionsを自動的にセットアップするようになりました。 また、2つの追加アプリケーションプロパティにアクセスできるようになります:

- `app.extend()` - Sanic Extensionsを設定するために使用されます。
- `app.ext` - アプリケーションにアタッチされている `Extend` インスタンスです。

プラグインの使用方法および作業方法の詳細については、[プラグイン ドキュメント](../plugins/sanic-ext/getting-started.md) を参照してください。