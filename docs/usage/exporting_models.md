<!--
As well as accessing model attributes directly via their names (e.g. `model.foobar`), models can be converted
and exported in a number of ways:
-->
モデルの名前（例: `model.foobar`）を介してモデル属性に直接アクセスするだけでなく、
モデルはさまざまな方法で変換およびエクスポートできます。

## `model.dict(...)`

<!--
This is the primary way of converting a model to a dictionary. Sub-models will be recursively converted to dictionaries.
-->
これは、モデルを辞書に変換する主な方法です。 サブモデルは再帰的に辞書に変換されます。

<!--
Arguments:
-->
引数:

<!--
* `include`: fields to include in the returned dictionary; see [below](#advanced-include-exclude)
-->
* `include`: 返却される辞書に含めるフィールド。[以下](#advanced-include-exclude)を参照

<!--
* `exclude`: fields to exclude from the returned dictionary; see [below](#advanced-include-exclude)
-->
* `exclude`: 返却される辞書から除外するフィールド。[以下](#advanced-include-exclude)を参照

<!--
* `by_alias`: whether field aliases should be used as keys in the returned dictionary; default `False`
-->
* `by_alias`: 返却される辞書のキーとしてフィールドエイリアスを使用するかどうか。デフォルトは `False`

<!--
* `exclude_unset`: whether fields which were not explicitly set when creating the model should
  be excluded from the returned dictionary; default `False`.
  Prior to **v1.0**, `exclude_unset` was known as `skip_defaults`; use of `skip_defaults` is now deprecated
-->
* `exclude_unset`: モデルの作成時に明示的に設定されなかったフィールドを、返却される辞書から除外するかどうか。
  デフォルトは `False`。 **v1.0** 以前は `exclude_unset` は `skip_defaults` と呼ばれていました。
  現在 `skip_defaults` は非推奨です。

<!--
* `exclude_defaults`: whether fields which are equal to their default values (whether set or otherwise) should
  be excluded from the returned dictionary; default `False`
-->
* `exclude_defaults`: デフォルト値と等しいフィールドを、(設定されているかどうかに関係なく)返却される辞書から除外するかどうか。デフォルトは `False`。

<!--
* `exclude_none`: whether fields which are equal to `None` should be excluded from the returned dictionary; default
  `False`
-->
* `exclude_none`: `None` に等しいフィールドを、返却される辞書から除外するかどうか。デフォルトは `False`。

<!--
Example:
-->
例:

```py
{!.tmp_examples/exporting_models_dict.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
## `dict(model)` and iteration
-->
## `dict(model)` と反復処理

<!--
*pydantic* models can also be converted to dictionaries using `dict(model)`, and you can also
iterate over a model's field using `for field_name, value in model:`. With this approach the raw field values are
returned, so sub-models will not be converted to dictionaries.
-->
*pydantic* モデルは `dict(model)` を使用して辞書に変換することもできますし、
`for field_name, value in model:` を使ってモデルのフィールドを反復処理することもできます。
このアプローチでは名前のフィールド値が返却されるため、サブモデルは辞書に変換されません。

<!--
Example:
-->
例:

```py
{!.tmp_examples/exporting_models_iterate.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

## `model.copy(...)`

<!--
`copy()` allows models to be duplicated, which is particularly useful for immutable models.
-->
`copy()` はモデルの複製を可能にします。これは特に不変モデルの場合に役に立ちます。

<!--
Arguments:
-->
引数:

<!--
* `include`: fields to include in the returned dictionary; see [below](#advanced-include-exclude)
-->
* `include`: 返却される辞書に含めるフィールド。[以下](#advanced-include-exclude)を参照

<!--
* `exclude`: fields to exclude from the returned dictionary; see [below](#advanced-include-exclude)
-->
* `exclude`: 返却される辞書から除外するフィールド。[以下](#advanced-include-exclude)を参照

<!--
* `update`: a dictionary of values to change when creating the copied model
-->
* `update`: コピーされたモデルを作成する際に変更する値の辞書

<!--
* `deep`: whether to make a deep copy of the new model; default `False`
-->
* `deep`: 新しいモデルのディープコピーを作成するかどうか。デフォルトは `False`。

<!--
Example:
-->
例:

```py
{!.tmp_examples/exporting_models_copy.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

## `model.json(...)`

<!--
The `.json()` method will serialise a model to JSON. Typically, `.json()` in turn calls `.dict()` and
serialises its result. (For models with a [custom root type](models.md#custom-root-types), after calling `.dict()`,
only the value for the `__root__` key is serialised)
-->
`.json()` メソッドはモデルを JSON にシリアライズします。通常 `.json()` は `.dict()` を呼び出し、その結果をシリアライズします。
(カスタムルートタイプを持つモデルの場合、`.dict()` を呼び出した後、`__root__` キーの値のみがシリアライズされます。)

<!--
Arguments:
-->
引数:

<!--
* `include`: fields to include in the returned dictionary; see [below](#advanced-include-exclude)
-->
* `include`: 返却される辞書に含めるフィールド。[以下](#advanced-include-exclude)を参照

<!--
* `exclude`: fields to exclude from the returned dictionary; see [below](#advanced-include-exclude)
-->
* `exclude`: 返却される辞書から除外するフィールド。[以下](#advanced-include-exclude)を参照

<!--
* `by_alias`: whether field aliases should be used as keys in the returned dictionary; default `False`
-->
* `by_alias`: 返却される辞書のキーとしてフィールドエイリアスを使用するかどうか。デフォルトは `False`

<!--
* `exclude_unset`: whether fields which were not set when creating the model and have their default values should
  be excluded from the returned dictionary; default `False`.
  Prior to **v1.0**, `exclude_unset` was known as `skip_defaults`; use of `skip_defaults` is now deprecated
-->
* `exclude_unset`: モデルの作成時に設定されておらず、デフォルト値を持つフィールドを、返却される辞書から除外するかどうか。
  デフォルトは `False`。 **v1.0** 以前は `exclude_unset` は `skip_defaults` と呼ばれていました。
  現在 `skip_defaults` は非推奨です。

<!--
* `exclude_defaults`: whether fields which are equal to their default values (whether set or otherwise) should
  be excluded from the returned dictionary; default `False`
-->
* `exclude_defaults`: デフォルト値と等しいフィールドを、(設定されているかどうかに関係なく)返却される辞書から除外するかどうか。デフォルトは `False`。

<!--
* `exclude_none`: whether fields which are equal to `None` should be excluded from the returned dictionary; default
  `False`
-->
* `exclude_none`: `None` に等しいフィールドを、返却される辞書から除外するかどうか。デフォルトは `False`。

<!--
* `encoder`: a custom encoder function passed to the `default` argument of `json.dumps()`; defaults to a custom
  encoder designed to take care of all common types
-->
* `encoder`: `json.dumps()` のデフォルト引数に渡されるカスタムエンコーダ関数。
  デフォルトは全ての一般的な型を処理するように設計されたカスタムエンコーダです。

<!--
* `**dumps_kwargs`: any other keyword arguments are passed to `json.dumps()`, e.g. `indent`.
-->
* `**dumps_kwargs`: その他のキーワード引数(例えば `indent`)は全て `json.dumps()` に渡されます。

<!--
*pydantic* can serialise many commonly used types to JSON (e.g. `datetime`, `date` or `UUID`) which would normally
fail with a simple `json.dumps(foobar)`.
-->
*pydantic* は `datetime`、`date`、`UUID`など、単純な `json.dumps(foobar)` では通常失敗するような一般的に使用される型を
JSON にシリアライズできます。

```py
{!.tmp_examples/exporting_models_json.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

### `json_encoders`

<!--
Serialisation can be customised on a model using the `json_encoders` config property; the keys should be types, and
the values should be functions which serialise that type (see the example below):
-->
シリアライズは、`json_encoders` プロパティを使用してモデル上でカスタマイズできます。
キーは型でなければならず、値はその型をシリアライズする関数でなければなりません(以下の例を参照)。

```py
{!.tmp_examples/exporting_models_json_encoders.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
By default, `timedelta` is encoded as a simple float of total seconds. The `timedelta_isoformat` is provided
as an optional alternative which implements ISO 8601 time diff encoding.
-->
デフォルトでは、`timedelta` は合計秒の単純な float としてエンコードされます。
`timedelta_isoformat` は ISO 8601 の時間差分エンコーディングを実装する代替として提供されます。

<!--
### Serialising subclasses
-->
### サブクラスのシリアライズ

<!--
!!! note
    New in version **v1.5**.
    
    Subclasses of common types were not automatically serialised to JSON before **v1.5**.
-->
!!! note
    **v1.5** の新機能。
    **v1.5** 以前は、共通型のサブクラスが自動的に JSON にシリアライズされませんでした。

<!--
Subclasses of common types are automatically encoded like their super-classes:
-->
共通型のサブクラスは、そのスーパークラスのように自動的にエンコードされます。

```py
{!.tmp_examples/exporting_models_json_subclass.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
### Custom JSON (de)serialisation
-->
### カスタム JSON のデシリアライズ

<!--
To improve the performance of encoding and decoding JSON, alternative JSON implementations
(e.g. [ujson](https://pypi.python.org/pypi/ujson)) can be used via the
`json_loads` and `json_dumps` properties of `Config`.
-->
JSON エンコードおよびデコードのパフォーマンスを向上するために、`Config` の `json_loads` プロパティと `json_dumps` プロパティを介して、
代替の JSON ([ujson](https://pypi.python.org/pypi/ujson) など)を使用することができます。

```py
{!.tmp_examples/exporting_models_ujson.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
`ujson` generally cannot be used to dump JSON since it doesn't support encoding of objects like datetimes and does
not accept a `default` fallback function argument. To do this, you may use another library like
[orjson](https://github.com/ijl/orjson).
-->
`ujson` は通常、datetimesオブジェクトのエンコードや `default` フォールバック関数の引数を受け入れないため、
JSON のダンプには使用できません。これを使うには [orjson](https://github.com/ijl/orjson) のような別のライブラリを使用します。

```py
{!.tmp_examples/exporting_models_orjson.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
Note that `orjson` takes care of `datetime` encoding natively, making it faster than `json.dumps` but
meaning you cannot always customise the encoding using `Config.json_encoders`.
-->
`orjson` は `datetime` エンコーディングをネイティブで処理するため `json.dumps` より高速ですが、
`Config.json_encoders` を使ってエンコーディングをカスタマイズできるとは限らないことに注意して下さい。

## `pickle.dumps(model)`

<!--
Using the same plumbing as `copy()`, *pydantic* models support efficient pickling and unpickling.
-->
`copy()` と同じ配管を使用して、*pydantic* モデルは pickle 化と非 pickle 化をサポートしています。

```py
{!.tmp_examples/exporting_models_pickle.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
## Advanced include and exclude
-->
## 高度な include と exclude

<!--
The `dict`, `json`, and `copy` methods support `include` and `exclude` arguments which can either be
sets or dictionaries. This allows nested selection of which fields to export:
-->
`dict`、`json`、`copy` メソッドは、セットまたは辞書となる `include` 引数と `exclude` 引数をサポートしています。
これにより、エクスポートするフィールドを入れ子にして選択できます。

```py
{!.tmp_examples/exporting_models_exclude1.py!}
```

<!--
The ellipsis (``...``) indicates that we want to exclude or include an entire key, just as if we included it in a set.
Of course, the same can be done at any depth level.
-->
省略記号(``...``)は、セットに含める場合と同様に、キー全体を除外または含めることを示します。
もちろん、どの深さレベルでも同じことができます。

<!--
Special care must be taken when including or excluding fields from a list or tuple of submodels or dictionaries.  In this scenario,
`dict` and related methods expect integer keys for element-wise inclusion or exclusion. To exclude a field from **every**
member of a list or tuple, the dictionary key `'__all__'` can be used as follows:
-->
リスト、サブモデルのタプル、辞書からフィールドを含めたり除外したりする場合は特別な注意が必要です。
このシナリオでは、`dict` と関連するメソッドは要素ごとの包含または除外のために、整数キーを想定しています。
リストやタプルの全てのメンバーからフィールドを除外するには、次のように辞書キー `__all__` を次のように使用します。

```py
{!.tmp_examples/exporting_models_exclude2.py!}
```

<!--
The same holds for the `json` and `copy` methods.
-->
同じことが `json` メソッドと `copy` メソッドにも当てはまります。
