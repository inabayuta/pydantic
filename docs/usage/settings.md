<!--
One of pydantic's most useful applications is settings management.
-->
pydantic の最も便利な機能の1つは、設定管理です。

<!--
If you create a model that inherits from `BaseSettings`, the model initialiser will attempt to determine
the values of any fields not passed as keyword arguments by reading from the environment. (Default values
will still be used if the matching environment variable is not set.)
-->
`BaseSettings` を継承するモデルを作成する場合、モデルイニシャライザーは、
キーワード引数として渡されていないフィールドの値を環境から読み取って決定しようとします。
(一致する環境変数が設定されていない場合でも、デフォルト値が使用されます。)

<!--
This makes it easy to:
-->
これは以下を簡単にします:

<!--
* Create a clearly-defined, type-hinted application configuration class
-->
* 明確に定義された型ヒント付きのアプリケーション構成クラスを作成する

<!--
* Automatically read modifications to the configuration from environment variables
-->
* 環境変数から設定の変更を自動的に読み込む

<!--
* Manually override specific settings in the initialiser where desired (e.g. in unit tests)
-->
* 必要に応じて(例: 単体テストなど)イニシャライザーの特定の設定を手動でオーバーライドする

<!--
For example:
-->
例えば:

```py
{!.tmp_examples/settings_main.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
## Environment variable names
-->
## 環境変数の名前

<!--
The following rules are used to determine which environment variable(s) are read for a given field:
-->
以下のルールは、与えられたフィールドに対してどの環境変数が読み込まれるかを決定するために使用されます。

<!--
* By default, the environment variable name is built by concatenating the prefix and field name.
    * For example, to override `special_function` above, you could use:

            export my_prefix_special_function='foo.bar'

    * Note 1: The default prefix is an empty string.
    * Note 2: Field aliases are ignored when building the environment variable name.
-->
* デフォルトでは、環境変数名はプレフィクスとフィールド名を連結して作成します。
    * 例えば、`special_function` をオーバーライドしたい場合は以下を使用します:
            export my_prefix_special_function='foo.bar'
    * 備考1: デフォルトのプレフィクスは空文字列です。
    * 備考2: 環境変数名をビルドする際はフィールドのエイリアスは無視されます。

<!--
* Custom environment variable names can be set in two ways:
    * `Config.fields['field_name']['env']` (see `auth_key` and `redis_dsn` above)
    * `Field(..., env=...)` (see `api_key` above)
* When specifying custom environment variable names, either a string or a list of strings may be provided.
    * When specifying a list of strings, order matters: the first detected value is used.
    * For example, for `redis_dsn` above, `service_redis_dsn` would take precedence over `redis_url`.
-->
* カスタム環境変数名は 2 つの方法で設定できます:
    * `Config.fields['field_name']['env']` (上記の `auth_key` と `redis_dsn` を参照)
    * `Field(..., env=...)` (上記の `api_key` を参照)

<!--
!!! warning
    Since **v1.0** *pydantic* does not consider field aliases when finding environment variables to populate settings
    models, use `env` instead as described above.

    To aid the transition from aliases to `env`, a warning will be raised when aliases are used on settings models
    without a custom env var name. If you really mean to use aliases, either ignore the warning or set `env` to
    suppress it.
-->

!!! warning
    *pydantic* **v1.0** では、設定モデルに埋め込む環境変数を見つける際にフィールドのエイリアスを考慮しないため、
    上記のように `env` を使用します。エイリアスから `env` への移行を支援するために、
    カスタム `env` 変数のない設定モデルでエイリアスを使用した場合に警告が表示されます。

<!--
Case-sensitivity can be turned on through the `Config`:
-->
大文字と小文字の区別は、`Config` を介してオンにできます:

```py
{!.tmp_examples/settings_case_sensitive.py!}
```

<!--
When `case_sensitive` is `True`, the environment variable names must match field names (optionally with a prefix),
so in this example
`redis_host` could only be modified via `export redis_host`. If you want to name environment variables
all upper-case, you should name attribute all upper-case too. You can still name environment variables anything
you like through `Field(..., env=...)`.
-->
`case_sensitive` が `True` の場合、環境変数名はフィールド名と一致する必要があるため(オプションでプレフィクス付きにできます)、
この例では、`redis_host` は `export_redis_host` を介してのみ変更できます。
環境変数の名前を全て大文字で付ける場合は、属性の名前も全て大文字の名前を付ける必要があります。
`Field(..., env=...)` を使用して、環境変数に任意の名前をつけることができます。

<!--
!!! note
    On Windows, python's `os` module always treats environment variables as case-insensitive, so the
    `case_sensitive` config setting will have no effect - settings will always be updated ignoring case.
-->
!!! note
    Windows では、Python の `os` モジュールは常に環境変数を大文字と小文字を区別せずに扱うため、
    `case_sensitive` を設定しても効果がありません。設定は常に大文字と小文字を区別せずに更新されます。

<!--
## Parsing environment variable values
-->
## 環境変数の値のパース

<!--
For most simple field types (such as `int`, `float`, `str`, etc.),
the environment variable value is parsed the same way it would
be if passed directly to the initialiser (as a string).
-->
ほとんどの単純なフィールド型(`int`、`float`、`str` など)の場合、
環境変数の値はイニシャライザーに(文字列として)直接渡される場合と同じ方法で解析されます。

<!--
Complex types like `list`, `set`, `dict`, and sub-models are populated from the environment
by treating the environment variable's value as a JSON-encoded string.
-->
`list`、`set`、`dict` などの複雑な型は、環境変数の値を JSON エンコードされた文字列として扱うことで、
環境から生成されます。

<!--
## Dotenv (.env) support
-->
## ドットエンブ (.env) サポート

<!--
!!! note
    dotenv file parsing requires [python-dotenv](https://pypi.org/project/python-dotenv/) to be installed.
    This can be done with either `pip install python-dotenv` or `pip install pydantic[dotenv]`.
-->
!!! note
    dotenv ファイルの解析には [python-dotenv](https://pypi.org/project/python-dotenv/) がインストールされている必要があります。
    `pip install python-dotenv` または `pip install pydantic[dotenv]` を使用して実行できます。

<!--
Dotenv files (generally named `.env`) are a common pattern that make it easy to use environment variables in a
platform-independent manner.
-->
ドットエンブファイル (一般に `.env` という名前) は、
プラットフォームに依存しない方法で環境変数を簡単に使用できるようにする一般的なパターンです。

<!--
A dotenv file follows the same general principles of all environment variables,
and looks something like:
-->
ドットエンブファイルは、全ての環境変数と同じ一般原則に従い、次のようになります:

```bash
# ignore comment
ENVIRONMENT="production"
REDIS_ADDRESS=localhost:6379
MEANING_OF_LIFE=42
MY_VAR='Hello world'
```

<!--
Once you have your `.env` file filled with variables, *pydantic* supports loading it in two ways:
-->
`.env` ファイルに変数を入力すると、*pydantic* は次の 2 つの方法でファイルの読み込みをサポートします:

<!--
**1.** setting `env_file` (and `env_file_encoding` if you don't want the default encoding of your OS) on `Config`
in a `BaseSettings` class:
-->
**1.** `BaseSettings` クラスの `Config` に `env_file` (OS のデフォルトエンコーディングが必要ない場合は `env_file_envoding`)を設定します。

```py
class Settings(BaseSettings):
    ...

    class Config:
        env_file = '.env'
        env_file_encoding = 'utf-8'
```

<!--
**2.** instantiating a `BaseSettings` derived class with the `_env_file` keyword argument
(and the `_env_file_encoding` if needed):
-->
**2.** キーワード引数 `_env_file` (必要な場合は `_env_file_encoding` も)を使用して、`BaseSettings` 派生クラスのインスタンスを作成します。

```py
settings = Settings(_env_file='prod.env', _env_file_encoding='utf-8')
```

<!--
In either case, the value of the passed argument can be any valid path or filename, either absolute or relative to the
current working directory. From there, *pydantic* will handle everything for you by loading in your variables and
validating them.
-->
いずれの場合も、渡される引数の値は絶対パス、ファイル名、または現在の作業ディレクトリからの相対パスをファイル名にできます。
そこから変数をロードしてバリデーションすることで、*pydantic* が全てを処理します。

<!--
Even when using a dotenv file, *pydantic* will still read environment variables as well as the dotenv file,
**environment variables will always take priority over values loaded from a dotenv file**.
-->
dotenv ファイルを使用している場合でも *pydantic* は環境変数を読み込みますが、
環境変数は常に dotenv ファイルから読み込まれた値よりも優先されます。

<!--
Passing a file path via the `_env_file` keyword argument on instantiation (method 2) will override
the value (if any) set on the `Config` class. If the above snippets were used in conjunction, `prod.env` would be loaded
while `.env` would be ignored.
-->
インスタンス化の際に `_env_file` キーワード引数を介してファイルパスを渡す(方法 2)と、
`Config` クラスに設定されている値(存在する場合)が上書きされます。
上記のスニペットを併用すると、`prod.env` が読み込まれ、`.env` は無視されます。

<!--
You can also use the keyword argument override to tell Pydantic not to load any file at all (even if one is set in
the `Config` class) by passing `None` as the instantiation keyword argument, e.g. `settings = Settings(_env_file=None)`.
-->
また、キーワード引数のオーバーライドを使用して、インスタンス化キーワード引数に `None` を渡すことで、
(例えば `Config` クラスでファイルが設定されている場合でも) Pydantic にファイルを全くロードしないように指示することもできます。
例えば `settings = Settings(_env_file=None)` のように設定します。

<!--
Because python-dotenv is used to parse the file, bash-like semantics such as `export` can be used which
(depending on your OS and environment) may allow your dotenv file to also be used with `source`,
see [python-dotenv's documentation](https://saurabh-kumar.com/python-dotenv/#usages) for more details.
-->
python-dotenv はファイルのパースに使用されるため、
エクスポートなどの bash ライクなセマンティクスを使用できます。
これにより、(OS と環境によっては) dotenv ファイルをソースと一緒に使用できます。
詳細については [python-dotenv のドキュメント](https://saurabh-kumar.com/python-dotenv/#usages)を参照して下さい。

<!--
## Field value priority
-->
## フィールド値のプロパティ

<!--
In the case where a value is specified for the same `Settings` field in multiple ways,
the selected value is determined as follows (in descending order of priority):
-->
同一の `Settings` フィールドに対して複数の方法で値を指定した場合、
選択された値は以下のように決定されます(優先度の高い順):

<!--
1. Arguments passed to the `Settings` class initialiser.
-->
1. `Settings` クラスのイニシャライザーに渡される引数

<!--
2. Environment variables, e.g. `my_prefix_special_function` as described above.
-->
2. 環境変数(例: 上記の `my_prefix_special_function`)の値

<!--
3. Variables loaded from a dotenv (`.env`) file.
-->
3. ドットエンブ(`.env`)ファイル からロードされた値

<!--
4. The default field values for the `Settings` model.
-->
4. `Settings` モデルのデフォルトフィールド値
