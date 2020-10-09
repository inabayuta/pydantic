<!--
Pydantic works well with [mypy](http://mypy-lang.org/) right [out of the box](usage/mypy.md).
-->
Pydantic は [mypy](http://mypy-lang.org/) で[細かい設定なしに](usage/mypy.md)上手く動作します。

<!--
However, Pydantic also ships with a mypy plugin that adds a number of important pydantic-specific
features to mypy that improve its ability to type-check your code.
-->
しかし Pydantic には mypy プラグインも付属しており、コードの型チェック機能を向上させるために多くの重要な Pydantic 固有の機能を追加しています。

<!--
For example, consider the following script:
-->
例えば、次のスクリプトを考えてみましょう:

```py
{!.tmp_examples/mypy_main.py!}
```

<!--
Without any special configuration, mypy catches one of the errors (see [here](usage/mypy.md) for usage instructions):
-->
特別な設定をしなくても、mypy はエラーを一つキャッチします(使用方法については[こちら](usage/mypy.md)を参照してください):

```
13: error: "Model" has no attribute "middle_name"
```

<!--
But [with the plugin enabled](#enabling-the-plugin), it catches both:
-->
しかし[プラグインを有効にする](#enabling-the-plugin)と、全てキャッチします:

```
13: error: "Model" has no attribute "middle_name"
16: error: Missing named argument "age" for "Model"
16: error: Missing named argument "list_of_ints" for "Model"
```

<!--
With the pydantic mypy plugin, you can fearlessly refactor your models knowing mypy will catch any mistakes
if your field names or types change.
-->
Pydantic の mypy プラグインを使えば、フィールド名や型が変わっても mypy がミスをキャッチしてくれることを知っているので、
恐れること無くモデルをリファクタリングできます。

<!--
There are other benefits too! See below for more details.
-->
他にも利益があります! 以下の詳細を見てください。

<!--
### Plugin Capabilities
-->
### プラグイン機能

<!--
#### Generate a signature for `Model.__init__`
-->
#### `Model.__init__` のシグネチャを生成

<!--
* Any required fields that don't have dynamically-determined aliases will be included as required
  keyword arguments.
-->
* 動的に決定されたエイリアスを持たない必須フィールドは、必須キーワード引数として含まれます。

<!--
* If `Config.allow_population_by_field_name=True`, the generated signature will use the field names,
  rather than aliases.
-->
* `Config.allow_population_by_field_name = True` の場合、生成されたシグネチャはエイリアスではなくフィールド名を使用します。

<!--
* For subclasses of [`BaseSettings`](usage/settings.md), all fields are treated as optional since they may be
  read from the environment.
-->
* [`BaseSettings`](usage/settings.md) のサブクラスでは、環境から読み込まれる可能性があるため、
  すべてのフィールドはオプションとして扱われます。

<!--
* If `Config.extra="forbid"` and you don't make use of dynamically-determined aliases, the generated signature
  will not allow unexpected inputs.
-->
* `config.extra="forbid"` で、動的に決定されたエイリアスを使用しない場合、生成されたシグネチャは予期しない入力を許可しません。

<!--
* **Optional:** If the [`init_forbid_extra` **plugin setting**](#plugin-settings) is set to `True`, unexpected inputs to
  `__init__` will raise errors even if `Config.extra` is not `"forbid"`.
-->
* **オプション**: [`init_forbid_extra` **プラグインの設定**](#plugin-settings)が `True` に設定されている場合、
  `Config.extra` が `"forbid"` でなくても、`__init__` への予期しない入力はエラーを発生させます。

<!--
* **Optional:** If the [`init_typed` **plugin setting**](#plugin-settings) is set to `True`, the generated signature
  will use the types of the model fields (otherwise they will be annotated as `Any` to allow parsing).
-->
* **オプション**: [`init_typed` **プラグインの設定**](#plugin-settings)が `True` に設定されている場合、
  生成されたシグネチャはモデルフィールドの型を使用します(そうでない場合は、解析を可能にするために `Any` としてアノテーションされます)。

<!--
#### Generate a typed signature for `Model.construct`
-->
#### `Model.construct` の型付けされたシグネチャを生成

<!--
* The [`construct`](usage/models.md#creating-models-without-validation) method is a faster alternative to `__init__`
  when input data is known to be valid and does not need to be parsed. But because this method performs no runtime
  validation, static checking is important to detect errors.
-->
* 入力データが有効であることがわかっていて解析する必要がない場合、[`construct`](usage/models.md#creating-models-without-validation) メソッドは `__init__` のより高速な代替手段です。
  ただし、このメソッドは実行時の検証を実行しないため、エラーを検出するには静的チェックが重要です。

<!--
#### Respect `Config.allow_mutation`
-->
#### `Config.allow_mutation` を尊重

<!--
* If `Config.allow_mutation` is `False`, you'll get a mypy error if you try to change
  the value of a model field; cf. [faux immutability](usage/models.md#faux-immutability).
-->
* `Config.allow_mutation` が `False` の場合、モデルフィールドの値を変更しようとすると mypy エラーが発生します。
  (例. [イミュータブルな偽](usage/models.md#faux-immutability))

<!--
#### Respect `Config.orm_mode`
-->
#### `Config.orm_mode` を尊重

<!--
* If `Config.orm_mode` is `False`, you'll get a mypy error if you try to call `.from_orm()`;
  cf. [ORM mode](usage/models.md#orm-mode-aka-arbitrary-class-instances)
-->
* `Config.orm_mode` が `False` の場合、`.from_orm()` をコールしようとすると mypy エラーが発生します。
  (例. [ORM モード](usage/models.md#orm-mode-aka-arbitrary-class-instances))

<!--
#### Generate a signature for `dataclasses`
-->
### `dataclasses` のシグネチャを生成

<!--
* classes decorated with [`@pydantic.dataclasess.dataclass`](usage/dataclasses.md) are type checked the same as standard python dataclasses
-->
* [`@pydantic.dataclasess.dataclass`](usage/dataclasses.md) でデコレータされたクラスは標準の Python データクラスと同じように型チェックされます。

<!--
* The `@pydantic.dataclasess.dataclass` decorator accepts a `config` keyword argument which has the same meaning as [the `Config` sub-class](usage/model_config.md).
-->
* `@pydantic.dataclasess.dataclass` デコレータは `Config` サブクラスと同じ意味を持つ `config` キーワード引数を受け入れます。

<!--
### Optional Capabilites:
-->
### オプション機能

<!--
#### Prevent the use of required dynamic aliases
-->
#### 必須の動的エイリアスの使用を防止

<!--
* If the [`warn_required_dynamic_aliases` **plugin setting**](#plugin-settings) is set to `True`, you'll get a mypy
  error any time you use a dynamically-determined alias or alias generator on a model with
  `Config.allow_population_by_field_name=False`.
-->
* [`warn_required_dynamic_aliases` **プラグイン設定**](#plugin-settings) が `True` に設定されている場合、
  `Config.allow_population_by_field_name=False` のモデルで動的に決定されたエイリアスまたはエイリアスジェネレータを使用すると mypy エラーが発生します。

<!--
* This is important because if such aliases are present, mypy cannot properly type check calls to `__init__`.
  In this case, it will default to treating all arguments as optional.
-->
* このようなエイリアスが存在する場合、`__init__` のコールを適切に型チェックできないため、これは重要です。
  この場合、すべての引数をオプションとして扱うことがデフォルトになります。

<!--
#### Prevent the use of untyped fields
-->
#### 型なしフィールドを防止

<!--
* If the [`warn_untyped_fields` **plugin setting**](#plugin-settings) is set to `True`, you'll get a mypy error
  any time you create a field on a model without annotating its type.
-->
* [`warn_untyped_fields` **プラグイン設定**](#plugin-settings) が `True` に設定されている場合、
  モデルに型をアノテーションせずにフィールドを作成すると mypy エラーが発生します。

<!--
* This is important because non-annotated fields may result in
  [**validators being applied in a surprising order**](usage/models.md#field-ordering).
-->
* アノテーションされていないフィールドでは[バリデータが意外な順序で適用される](usage/models.md#field-ordering)可能性があるため、これは重要です。

<!--
* In addition, mypy may not be able to correctly infer the type of the field, and may miss
  checks or raise spurious errors.
-->
* さらに、mypyはフィールドのタイプを正しく推測できない可能性があり、
  チェックし損ねたり、誤ったエラーを発生させたりする可能性があります。

<!--
### Enabling the Plugin
-->
### プラグインを有効化する

<!--
To enable the plugin, just add `pydantic.mypy` to the list of plugins in your
[mypy config file](https://mypy.readthedocs.io/en/latest/config_file.html)
(this could be `mypy.ini` or `setup.cfg`).
-->
プラグインを有効にするには、[mypy 構成ファイル](https://mypy.readthedocs.io/en/latest/config_file.html)
(これは `mypy.ini` または `setup.cfg` になります)のプラグインリストに `pydantic.mypy` を追加するだけです。

<!--
To get started, all you need to do is create a `mypy.ini` file with following contents:
-->
開始するには、次の内容の `mypy.ini` ファイルを作成するだけです。

```ini
[mypy]
plugins = pydantic.mypy
```

<!--
See the [mypy usage](usage/mypy.md) and [plugin configuration](#configuring-the-plugin) docs for more details.
-->
より詳細な情報は [mypy の使い方](usage/mypy.md) と [プラグイン構成](#configuring-the-plugin) を見てください。

<!--
### Plugin Settings
-->
### プラグイン設定

<!--
The plugin offers a few optional strictness flags if you want even stronger checks:
-->
さらに強力なチェックが必要な場合、プラグインはいくつかのオプションの厳密なフラグを提供します。

* `init_forbid_extra`
<!--
    If enabled, disallow extra arguments to the `__init__` call even when `Config.extra` is not `"forbid"`.
-->
有効にすると、`Config.extra` が ` "forbid" ` でない場合でも `__init__` 呼び出しへの追加の引数を許可しません。

* `init_typed`
<!--
    If enabled, include the field types as type hints in the generated signature for the `__init__` method.
-->
有効にすると、`__init__` メソッド用に生成されたシグネチャに型ヒントとしてフィールドの型を含めます。

<!--
    This means that you'll get mypy errors if you pass an argument that is not already the right type to
    `__init__`, even if parsing could safely convert the type.
-->
これは、たとえ安全に型をパースできたとしても正しい型でない引数を `__init__` に渡すと mypy のエラーが発生することを意味します。

* `warn_required_dynamic_aliases`
<!--
    If enabled, raise a mypy error whenever a model is created for which
    calls to its `__init__` or `construct` methods require the use of aliases that cannot be statically determined.
    This is the case, for example, if `allow_population_by_field_name=False` and the model uses an alias generator.
-->
有効にすると、モデルが作成されたときに `__init__` または `construct` メソッドの呼び出しで静的に決定できないエイリアスの使用を必要とする場合に、mypy エラーが発生します。
これは例えば、`allow_population_by_field_name=False` で、モデルがエイリアスジェネレーターを使用している場合に起こります。

* `warn_untyped_fields`
<!--
    If enabled, raise a mypy error whenever a field is declared on a model without explicitly specifying its type.
-->
有効にすると、モデル上で型を明示的に指定せずにフィールドを宣言する場合に mypy エラーが発生します。

<!--
#### Configuring the Plugin
-->
#### プラグインの設定

<!--
To change the values of the plugin settings, create a section in your mypy config file called `[pydantic-mypy]`,
and add any key-value pairs for settings you want to override.
-->
プラグイン設定の値を変更するには mypy 設定ファイルに `[pydantic-mypy]` というセクションを作成し、
オーバーライドしたい設定のキーと値のペアを追加します。

<!--
A `mypy.ini` file with all plugin strictness flags enabled (and some other mypy strictness flags, too) might look like:
-->
全てのプラグインの厳密フラグ(および他のいくつかの mypy 厳密フラグ)を有効にした `mypy.ini` ファイルは次のようになります:

```ini
[mypy]
plugins = pydantic.mypy

follow_imports = silent
strict_optional = True
warn_redundant_casts = True
warn_unused_ignores = True
disallow_any_generics = True
check_untyped_defs = True
no_implicit_reexport = True

# for strict mypy: (this is the tricky one :-))
disallow_untyped_defs = True

[pydantic-mypy]
init_forbid_extra = True
init_typed = True
warn_required_dynamic_aliases = True
warn_untyped_fields = True
```
