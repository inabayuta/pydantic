[![CI](https://github.com/samuelcolvin/pydantic/workflows/CI/badge.svg?event=push)](https://github.com/samuelcolvin/pydantic/actions?query=event%3Apush+branch%3Amaster+workflow%3ACI)
[![Coverage](https://codecov.io/gh/samuelcolvin/pydantic/branch/master/graph/badge.svg)](https://codecov.io/gh/samuelcolvin/pydantic)
[![pypi](https://img.shields.io/pypi/v/pydantic.svg)](https://pypi.python.org/pypi/pydantic)
[![CondaForge](https://img.shields.io/conda/v/conda-forge/pydantic.svg)](https://anaconda.org/conda-forge/pydantic)
[![downloads](https://img.shields.io/pypi/dm/pydantic.svg)](https://pypistats.org/packages/pydantic)
[![license](https://img.shields.io/github/license/samuelcolvin/pydantic.svg)](https://github.com/samuelcolvin/pydantic/blob/master/LICENSE)

{!.version.md!}

<!--
Data validation and settings management using python type annotations.
-->
Python 型アノテーションを使用したデータバリデーションと設定管理。

<!--
*pydantic* enforces type hints at runtime, and provides user friendly errors when data is invalid.
-->
*pydantic* は実行時に型ヒントを適用し、データが無効な場合にユーザーフレンドリーなエラーを提供します。

<!--
Define how data should be in pure, canonical python; validate it with *pydantic*.
-->
純粋で標準的な Python を用いてデータがどのようにあるべきかを定義し、*pydantic* でバリデーションします。

<!--
## Example
-->
## 例

```py
{!.tmp_examples/index_main.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しているので、"そのまま"実行してください)_

<!--
What's going on here:
-->
ここで何が起きているか:

<!--
* `id` is of type int; the annotation-only declaration tells *pydantic* that this field is required. Strings,
  bytes or floats will be coerced to ints if possible; otherwise an exception will be raised.
-->
* `id` は int 型です。アノテーションのみの宣言は、このフィールドが必須であることを *pydantic* に伝えます。
  文字列、バイト、または浮動小数点は可能であれば int に強制変換されます。

<!--
* `name` is inferred as a string from the provided default; because it has a default, it is not required.
-->
* `name` は提供されたデフォルトから文字列として推論されます。デフォルトがあるため、必須ではありません。

<!--
* `signup_ts` is a datetime field which is not required (and takes the value ``None`` if it's not supplied).
  *pydantic* will process either a unix timestamp int (e.g. `1496498400`) or a string representing the date & time.
-->
* `signup_ts` は datetime フィールドです (指定されていない場合は `None` 値を取ります)。
  *pydantic* は Unix タイムスタンプ (例: `1496498400`) または日付と時刻を表す文字列のいずれかを処理します。

<!--
* `friends` uses python's typing system, and requires a list of integers. As with `id`, integer-like objects
  will be converted to integers.
-->
* `friends` は Python の typing を使用しており、数値のリストが必要です。`id` のように、
  整数のようなオブジェクトは整数に変換されます。

<!--
If validation fails pydantic will raise an error with a breakdown of what was wrong:
-->
バリデーションが失敗した場合、pydantic は何が悪かったかの内訳とともにエラーを発生させます。

```py
{!.tmp_examples/index_error.py!}
```

<!--
outputs:
-->
出力:

```json
{!.tmp_examples/index_error.json!}
```

<!--
## Rationale
-->
## 根拠

<!--
So *pydantic* uses some cool new language features, but why should I actually go and use it?
-->
それで、*pydantic* はいくつかのクールな新しい言語機能を使用していますが、なぜ私が使う必要があるのですか？

<!--
**plays nicely with your IDE/linter/brain**
-->
**あなたのIDE/リンター/脳とうまく連携します**
<!--
: There's no new schema definition micro-language to learn. If you know how to use python type hints, 
  you know how to use *pydantic*. Data structures are just instances of classes you define with type annotations, 
  so auto-completion, linting, [mypy](usage/mypy.md), IDEs (especially [PyCharm](pycharm_plugin.md)), 
  and your intuition should all work properly with your validated data.
-->
: 新しいスキーマ定義のマイクロ言語を学ぶ必要はありません。Python の型ヒントの使い方を知っていれば、あなたは *pydantic* の使い方を知っています。
  データ構造は型アノテーションで定義したクラスの単なるインスタンスに過ぎないので、自動補完、リンティング、[mypy](usage/mypy.md)、
  IDE(特に[PyCharm](pycharm_plugin.md))、そしてあなたの直感は検証済みのデータで全て適切に機能するはずです。

<!--
**dual use**
-->
**兼用**

<!--
: *pydantic's* [BaseSettings](usage/settings.md) class allows *pydantic* to be used in both a "validate this request
  data" context and in a "load my system settings" context. The main differences are that system settings can
  be read from environment variables, and more complex objects like DSNs and python objects are often required.
-->
: *pydantic* の [BaseSettings](usage/settings.md) クラスは、*pydantic* に"このリクエストデータをバリデートする"というコンテキストと、
  "システム設定をロードする"というコンテキスト両方を与えます。主な違いは環境変数から読み込めることと、
  DSN や Python オブジェクトのようなより複雑なオブジェクトに多く必要とされることです。

<!--
**fast**
-->
**高速**

<!--
: In [benchmarks](benchmarks.md) *pydantic* is faster than all other tested libraries.
-->
: [ベンチマーク](benchmarks.md)では、*pydantic* は他の全てのテスト済みライブラリよりも高速です。

<!--
**validate complex structures**
-->
**複雑な構造をバリデートする**

<!--
: use of [recursive *pydantic* models](usage/models.md#recursive-models), `typing`'s 
  [standard types](usage/types.md#standard-library-types) (e.g. `List`, `Tuple`, `Dict` etc.) and 
  [validators](usage/validators.md) allow
  complex data schemas to be clearly and easily defined, validated, and parsed.
-->
: [再帰的な *pydantic* モデル](usage/models.md#recursive-models)、typing の[標準タイプ](usage/types.md#standard-library-types)(例: `List`、`Tuple`、`Dict` など)、
  および[バリデータ](usage/validators.md)を使用すると、複雑なデータスキーマを明確かつ簡単に定義、検証、および解析することができます。

<!--
**extensible**
-->
**拡張可能**

<!--
: *pydantic* allows [custom data types](usage/types.md#custom-data-types) to be defined or you can extend validation 
  with methods on a model decorated with the [`validator`](usage/validators.md) decorator.
-->
*pydantic* では、[カスタムデータ型](usage/types.md#custom-data-types)を定義したり、
[`validator`](usage/validators.md) デコレータで装飾されたモデルを使用してバリデーションを拡張することができます。

<!--
**dataclasses integration**
-->
**データクラスの統合**

<!--
: As well as `BaseModel`, *pydantic* provides
  a [`dataclass`](usage/dataclasses.md) decorator which creates (almost) vanilla python dataclasses with input
  data parsing and validation.
-->
`BaseModel` と同様に、*pydantic* は [`dataclass`](usage/dataclasses.md) デコレータを提供し、
入力データの解析とバリデーションを行う(ほぼ)バニラの Python データクラスを作成します。

<!--
## Using Pydantic
-->
## Pydantic を使用する

<!--
Hundreds of organisations and packages are using *pydantic*, including:
-->
何百もの組織やパッケージが *pydantic* を使用しています。

[FastAPI](https://fastapi.tiangolo.com/)
<!--
: a high performance API framework, easy to learn,
  fast to code and ready for production, based on *pydantic* and Starlette.
-->
: *pydantic* と Starlette に基づいた、習得が容易で、コードを素早く書ける、
  本番環境に対応した高性能 API フレームワーク

[Project Jupyter](https://jupyter.org/)
<!--
: developers of the Jupyter notebook are using *pydantic* 
  [for subprojects](https://github.com/samuelcolvin/pydantic/issues/773).
-->
: Jupyter Notebook の開発者はサブプロジェクトで *pydantic* を使用しています。

**Microsoft**
<!--
: are using *pydantic* (via FastAPI) for 
  [numerous services](https://github.com/tiangolo/fastapi/pull/26#issuecomment-463768795), some of which are 
  "getting integrated into the core Windows product and some Office products."
-->
: *pydantic* を(FastAPI経由で)使用しており、そのうちのいくつかは、"Windows のコア製品と Office 製品に統合されるようになってきています"。

**Amazon Web Services**
<!--
: are using *pydantic* in [gluon-ts](https://github.com/awslabs/gluon-ts), an open-source probabilistic time series
  modeling library.
-->
: オープンソースの確率的時系列モデリングライブラリ [gluon-ts](https://github.com/awslabs/gluon-ts) で *pydantic* を使用しています。

**The NSA**
<!--
: are using *pydantic* in [WALKOFF](https://github.com/nsacyber/WALKOFF), an open-source automation framework.
-->
: オープンソースの自動化フレームワーク [WALKOFF](https://github.com/nsacyber/WALKOFF) で *pydantic* を使用しています。

**Uber**
<!--
: are using *pydantic* in [Ludwig](https://github.com/uber/ludwig), an open-source TensorFlow wrapper.
-->
: オープンソースの TensorFlow ラッパー [Ludwig](https://github.com/uber/ludwig) で *pydantic* を使用しています。

**Cuenca**
<!--
: are a Mexican neobank that uses *pydantic* for several internal
  tools (including API validation) and for open source projects like
  [stpmex](https://github.com/cuenca-mx/stpmex-python), which is used to process real-time, 24/7, inter-bank
  transfers in Mexico.
-->
: メキシコのネット銀行 Cuenca は、いくつかの(API検証を含む)内部ツール、
  およびメキシコで24時間年中無休のリアルタイムの銀行感想金を処理するために使用される [stpmex](https://github.com/cuenca-mx/stpmex-python) 
  などのオープンソースプロジェクトに *pydantic* を使用しています。

[The Molecular Sciences Software Institute](https://molssi.org)
<!--
: are using *pydantic* in [QCFractal](https://github.com/MolSSI/QCFractal), a massively distributed compute framework
  for quantum chemistry.
-->
: 量子化学用の大規模分散コンピューティングのフレームワーク [QCFractal](https://github.com/MolSSI/QCFractal) で *pydantic* を使用しています。

[Reach](https://www.reach.vote)
<!--
: trusts *pydantic* (via FastAPI) and [*arq*](https://github.com/samuelcolvin/arq) (Samuel's excellent
  asynchronous task queue) to reliably power multiple mission-critical microservices.
-->
: 複数のミッションクリティカルなマイクロサービスを確実に動かすために、
  *pydantic* (FastAPI経由) と [*arq*](https://github.com/samuelcolvin/arq) (Samuel の優れた非同期タスクキュー)を信頼しています。

<!--
For a more comprehensive list of open-source projects using *pydantic* see the 
[list of dependents on github](https://github.com/samuelcolvin/pydantic/network/dependents).
-->
*pydantic* を使用したオープンソースプロジェクトのより包括的なリストは、
[Githubの依存関係リスト](https://github.com/samuelcolvin/pydantic/network/dependents)を参照してください。

<!--
## Discussion of Pydantic
-->
## Pydantic のディスカッション

<!--
Podcasts and videos discussing pydantic.
-->
ポッドキャストやビデオで Pydantic が議論されています。

[Podcast.\_\_init\_\_](https://www.pythonpodcast.com/pydantic-data-validation-episode-263/){target=_blank}
<!--
: Discussion about where *pydantic* came from and ideas for where it might go next with 
  Samuel Colvin the creator of pydantic.
-->
: *pydantic* がどこから来たのか、そして次の方向性のアイデアについて pydantic の作者である Samuel Colvin とディスカッション

[Python Bytes Podcast](https://pythonbytes.fm/episodes/show/157/oh-hai-pandas-hold-my-hand){target=_blank}
<!--
: "*This is a sweet simple framework that solves some really nice problems... Data validations and settings management 
  using python type annotations, and it's the python type annotations that makes me really extra happy... It works 
  automatically with all the IDE's you already have.*" --Michael Kennedy
-->
: "*これはいくつかの問題を解決する本当に素晴らしいシンプルなフレームワークです…… 
  Python 型アノテーションを使用したデータのバリデーションと設定管理、そして私を本当に幸せにするのは Python アノテーションです……
  あなたが既に持っている全ての IDE で自動的に動作します。*" --Michael Kennedy

[Python pydantic Introduction – Give your data classes super powers](https://www.youtube.com/watch?v=WJmqgJn9TXg){target=_blank}
<!--
: a talk by Alexander Hultnér originally for the Python Pizza Conference introducing new users to pydantic and walking 
  through the core features of pydantic.
-->
: Alexander Hultnér による Python Pizza Conference の講演で、新しいユーザーに pydantic を紹介し、コア機能についてウォークスルーを行いました。
