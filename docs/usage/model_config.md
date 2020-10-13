<!--
Behaviour of pydantic can be controlled via the `Config` class on a model.
-->
pydantic の動作はモデル上の `Config` クラスを介して制御することができます。

<!--
Options:
-->
オプション:

<!--
**`title`**
: the title for the generated JSON Schema
-->
**`title`**
: 生成された JSON スキーマのタイトル

<!--
**`anystr_strip_whitespace`**
: whether to strip leading and trailing whitespace for str & byte types (default: `False`)
-->
**`anystr_strip_whitespace`**
: str 型と byte 型の先頭と末尾の空白を削除するかどうか(デフォルト: `False`)

<!--
**`min_anystr_length`**
: the min length for str & byte types (default: `0`)
-->
**`min_anystr_length`**
: str 型と byte 型の長さの最小値(デフォルト: `0`)

<!--
**`max_anystr_length`**
: the max length for str & byte types (default: `2 ** 16`)
-->
**`max_anystr_length`**
: str 型と byte 型の長さの最大値(デフォルト: `2 ** 16`)

<!--
**`validate_all`**
: whether to validate field defaults (default: `False`)
-->
**`validate_all`**
: フィールドのデフォルトを検証するかどうか(デフォルト: `False`)

<!--
**`extra`**
: whether to ignore, allow, or forbid extra attributes during model initialization. Accepts the string values of
  `'ignore'`, `'allow'`, or `'forbid'`, or values of the `Extra` enum (default: `Extra.ignore`).
  `'forbid'` will cause validation to fail if extra attributes are included, `'ignore'` will silently ignore any extra attributes,
  and `'allow'` will assign the attributes to the model.
-->
**`extra`**
: モデルの初期化中に追加の属性を無視するか、許可するか、禁止するかどうか。
  `'ignore'`、`'allow'`、`'forbid'` のいずれかの文字列、または `Extra` 列挙型の値を受け取ります(デフォルト: `Extra.ignore`)。
  `'forbid'` は追加の属性が含まれている場合に検証を失敗させ、`'ignore'` は追加の属性を黙って無視し、`'allow'` は追加の属性をモデルに割り当てます。

<!--
**`allow_mutation`**
: whether or not models are faux-immutable, i.e. whether `__setattr__` is allowed (default: `True`)
-->
**`allow_mutation`**
: モデルがイミュータブルな偽でないかどうか、例えば `__setattr__` が許可されているかどうか(デフォルト: `True`)。

<!--
**`use_enum_values`**
: whether to populate models with the `value` property of enums, rather than the raw enum.
  This may be useful if you want to serialise `model.dict()` later (default: `False`)
-->
**`use_enum_values`**
: 生の列挙型ではなく、列挙型の `value` プロパティをモデルに設定するかどうか。
  これは後で `model.dict()` を後でシリアライズしたい場合に役立ちます(デフォルト: `False`)。

<!--
**`fields`**
: a `dict` containing schema information for each field; this is equivalent to
  using [the `Field` class](schema.md) (default: `None`)
-->
**`fields`**
: 各フィールドのスキーマ情報を含む `dict`。これは [`Field` クラス](schema.md)を使うのと同じです(デフォルト: `None`)。

<!--
**`validate_assignment`**
: whether to perform validation on *assignment* to attributes (default: `False`)
-->
**`validate_assignment`**
: 属性への*割当*に対してバリデーションを行うかどうか(デフォルト: `False`)。

<!--
**`allow_population_by_field_name`**
: whether an aliased field may be populated by its name as given by the model
  attribute, as well as the alias (default: `False`)
-->
**`allow_population_by_field_name`**
: エイリアスフィールドに、モデル属性で指定された名前とエイリアスを入力できるかどうか(デフォルト: `False`)。

<!--
!!! note
    The name of this configuration setting was changed in **v1.0** from
    `allow_population_by_alias` to `allow_population_by_field_name`.
-->
!!! note
    この構成設定の名前は、**v1.0** で `allow_population_by_alias` から `allow_population_by_field_name` に変更されました。

<!--
**`error_msg_templates`**
: a `dict` used to override the default error message templates.
  Pass in a dictionary with keys matching the error messages you want to override (default: `{}`)
-->
**`error_msg_templates`**: デフォルトのエラーメッセージテンプレートを上書きするために使用される `dict`。
上書きするエラーメッセージに一致するキーを使用して辞書を渡します(デフォルト: {})

<!--
**`arbitrary_types_allowed`**
: whether to allow arbitrary user types for fields (they are validated simply by
  checking if the value is an instance of the type). If `False`, `RuntimeError` will be
  raised on model declaration (default: `False`). See an example in
  [Field Types](types.md#arbitrary-types-allowed).
-->
**`arbitrary_types_allowed`**:
フィールドに任意のユーザー型を許可するかどうかを指定します(値が型のインスタンスであるかどうかをチェックするだけで検証されます)。
`False` の場合、モデル宣言時に `RuntimeError` が発生します(デフォルトは `False`)。
[Field の型](types.md#arbitrary-types-allowed)の例を参照してください。

<!--
**`orm_mode`**
: whether to allow usage of [ORM mode](models.md#orm-mode)
-->
**`orm_mode`**:
[ORM モード](models.md#orm-mode)の使用を許可するかどうか

<!--
**`getter_dict`**
: a custom class (which should inherit from `GetterDict`) to use when decomposing ORM classes for validation,
  for use with `orm_mode`
-->
**`getter_dict`**:
`orm_mode` を使用する際、検証のために ORM クラスを分解して使用するカスタムクラス(`GetterDict` から継承する必要があります)。

<!--
**`alias_generator`**
: a callable that takes a field name and returns an alias for it
-->
**`alias_generator`**:
フィールド名を受け取り、そのエイリアスを返す呼び出し可能オブジェクト

<!--
**`keep_untouched`**
: a tuple of types (e.g. descriptors) for a model's default values that should not be changed during model creation and will
not be included in the model schemas. **Note**: this means that attributes on the model with *defaults of this type*, not *annotations of this type*, will be left alone.
-->
**`keep_untouched`**:
モデルの作成中に変更してはならず、モデルスキーマに含まれない、モデルのデフォルト値の型のタプル(例: 記述子)。
注: これはこの型の*アノテーションではなく*、この型のデフォルトを持つモデルの属性がそのまま残されることを意味します。

<!--
**`schema_extra`**
: a `dict` used to extend/update the generated JSON Schema, or a callable to post-process it; see [Schema customization](schema.md#schema-customization)
-->
**`schema_extra`**:
生成された JSON スキーマを拡張/更新するために使用される辞書、またはそれを後処理するための呼び出し可能オブジェクト。
[スキーマのカスタマイズ](schema.md#schema-customization)を参照してください。

<!--
**`json_loads`**
: a custom function for decoding JSON; see [custom JSON (de)serialisation](exporting_models.md#custom-json-deserialisation)
-->
**`json_loads`**:
JSON デコードのためのカスタム関数。
[カスタム JSON のシリアライゼーションとデシリアライゼーション](exporting_models.md#custom-json-deserialisation)を参照してください。

<!--
**`json_dumps`**
: a custom function for encoding JSON; see [custom JSON (de)serialisation](exporting_models.md#custom-json-deserialisation)
-->
**`json_dumps`**:
JSON エンコードのためのカスタム関数。
[カスタム JSON のシリアライゼーションとデシリアライゼーション](exporting_models.md#custom-json-deserialisation)を参照してください。

<!--
**`json_encoders`**
: a `dict` used to customise the way types are encoded to JSON; see [JSON Serialisation](exporting_models.md#modeljson)
-->
**`json_encoders`**:
型を JSON にエンコードする際の設定に使用される `dict`。
[JSON シリアライゼーション](exporting_models.md#modeljson)を参照してください。

```py
{!.tmp_examples/model_config_main.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
Similarly, if using the `@dataclazss` decorator:
-->
同様に、`@dataclass` デコレータを使用する場合:

```py
{!.tmp_examples/model_config_dataclass.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
## Alias Generator
-->
## エイリアスジェネレータ

<!--
If data source field names do not match your code style (e. g. CamelCase fields),
you can automatically generate aliases using `alias_generator`:
-->
データソースフィールド名がコードスタイル(例: CamelCase フィールド)と一致しない場合は、
`alias_generator` を使用してエイリアスを自動的に生成できます。

```py
{!.tmp_examples/model_config_alias_generator.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
Here camel case refers to ["upper camel case"](https://en.wikipedia.org/wiki/Camel_case) aka pascal case
e.g. `CamelCase`. If you'd like instead to use lower camel case e.g. `camelCase`,
it should be trivial to modify the `to_camel` function above.
-->
ここでキャメルケースは ["アッパーキャメルケース"](https://en.wikipedia.org/wiki/Camel_case)、
別名パスカルケース(例: CamelCase) を指します。もし `camelCase` のように小文字を使いたい場合は、
上の関数 `to_camel` を修正すれば簡単です。

<!--
## Alias Precedence
-->
## エイリアスの優先順位

<!--
!!! warning
    Alias priority logic changed in **v1.4** to resolve buggy and unexpected behaviour in previous versions.
    In some circumstances this may represent a **breaking change**,
    see [#1178](https://github.com/samuelcolvin/pydantic/issues/1178) and the precedence order below for details.
-->
!!! warning
    以前のバージョンでのバグや予期しない動作を解決するために、**v1.4** でエイリアス優先度ロジックが変更されました。
    状況によっては、これは**破壊的な変更**になります。
    詳細は [#1178](https://github.com/samuelcolvin/pydantic/issues/1178) および以下の優先順位を参照してください。

<!--
In the case where a field's alias may be defined in multiple places,
the selected value is determined as follows (in descending order of priority):
-->
フィールドのエイリアスが複数箇所で定義されている場合、
選択された値は以下のように決定されます(優先度の高い順):

<!--
1. Set via `Field(..., alias=<alias>)`, directly on the model
2. Defined in `Config.fields`, directly on the model
3. Set via `Field(..., alias=<alias>)`, on a parent model
4. Defined in `Config.fields`, on a parent model
5. Generated by `alias_generator`, regardless of whether it's on the model or a parent
-->
1. モデル上で直接 `Field(..., alias=<alias>)` で設定した
2. モデル上で直接 `Config.fields` で定義した
3. 親モデル上で直接 `Field(..., alias=<alias>)` で設定した
4. 親モデル上で直接 `Config.fields` で定義した
5. モデル上か親モデルかに関わらず、`alias_generator` で生成した

<!--
!!! note
    This means an `alias_generator` defined on a child model **does not** take priority over an alias defined
    on a field in a parent model.
-->
!!! note
    これは、子モデルに定義された `alias_generator` が、
    親モデルのフィールドに定義されたエイリアスよりも優先**されない**ことを意味します。

<!--
For example:
-->
例えば:

```py
{!.tmp_examples/model_config_alias_precedence.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_
