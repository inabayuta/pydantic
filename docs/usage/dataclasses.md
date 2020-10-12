<!--
If you don't want to use pydantic's `BaseModel` you can instead get the same data validation on standard
[dataclasses](https://docs.python.org/3/library/dataclasses.html) (introduced in python 3.7).
-->
Pydantic の `BaseModel` を使用したくない場合は、
代わりに標準の[データクラス](https://docs.python.org/3/library/dataclasses.html)(Python 3.7 から導入)で同じデータバリデーションができます。

<!--
Dataclasses work in python 3.6 using the [dataclasses backport package](https://github.com/ericvsmith/dataclasses).
-->
データクラスは Python 3.6 では[バックポートパッケージ](https://github.com/ericvsmith/dataclasses)を使って動作します。

```py
{!.tmp_examples/dataclasses_main.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
!!! note
    Keep in mind that `pydantic.dataclasses.dataclass` is a drop-in replacement for `dataclasses.dataclass`
    with validation, **not** a replacement for `pydantic.BaseModel` (with a small difference in how [initialization hooks](#initialize-hooks) work). There are cases where subclassing
    `pydantic.BaseModel` is the better choice.

    For more information and discussion see
    [samuelcolvin/pydantic#710](https://github.com/samuelcolvin/pydantic/issues/710).
-->
!!! note
    `pydantic.dataclasses.dataclass` は `dataclasses.dataclass` にバリデーション機能を持たせた互換品であり、
    `pydantic.BaseModel` の置き換え**ではない**ことに注意してください(初期化フックの動作方法に若干違いがあります)。
    `pydantic.BaseModel` をサブクラス化する方が適切な場合もあります。

<!--
You can use all the standard pydantic field types, and the resulting dataclass will be identical to the one
created by the standard library `dataclass` decorator.
-->
すべての標準 pydantic フィールドタイプを使用でき、
結果のデータクラスは標準ライブラリの `dataclass` デコレータによって作成されたものと同じになります。

<!--
The underlying model and its schema can be accessed through `__pydantic_model__`.
Also, fields that require a `default_factory` can be specified by a `dataclasses.field`.
-->
基となるモデルとそのスキーマには、`__pydantic_model__` からアクセスできます。
また、`default_factory` を必要とするフィールドは `dataclass.field` で指定できます。 

```py
{!.tmp_examples/dataclasses_default_schema.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
`pydantic.dataclasses.dataclass`'s arguments are the same as the standard decorator, except one extra
keyword argument `config` which has the same meaning as [Config](model_config.md).
-->
`pydantic.dataclasses.dataclass` の引数は、
[Config](model_config.md) と同じ意味の追加キーワード引数 `config` を除き、標準デコレータと同じです。

<!--
!!! warning
    After v1.2, [The Mypy plugin](/mypy_plugin.md) must be installed to type check pydantic dataclasses.
-->
!!! warning
    v1.2 以降は、pydantic データクラスの型チェックをするために [Mypy プラグイン](/mypy_plugin.md)をインストールする必要があります。

<!--
For more information about combining validators with dataclasses, see
[dataclass validators](validators.md#dataclass-validators).
-->
バリデータとデータクラスの組み合わせについては、
[データクラスバリデータ](validators.md#dataclass-validators)を参照してください。

<!--
## Nested dataclasses
-->
## ネストされたデータクラス

<!--
Nested dataclasses are supported both in dataclasses and normal models.
-->
ネストされたデータクラスは、データクラスと標準モデル両方をサポートします。

```py
{!.tmp_examples/dataclasses_nested.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
Dataclasses attributes can be populated by tuples, dictionaries or instances of the dataclass itself.
-->
データクラスの属性は、データクラス自体のタプル、辞書、またはインスタンスによって設定できます。

<!--
## Initialize hooks
-->
## フックの初期化

<!--
When you initialize a dataclass, it is possible to execute code *after* validation
with the help of `__post_init_post_parse__`. This is not the same as `__post_init__`, which executes
code *before* validation.
-->
データクラスを初期化する際に、`__post_init_post_parse__` を使用して、検証後にコードを実行することができます。
これは検証前にコードを実効する `__post_init__` とは異なります。

```py
{!.tmp_examples/dataclasses_post_init_post_parse.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
Since version **v1.0**, any fields annotated with `dataclasses.InitVar` are passed to both `__post_init__` *and*
`__post_init_post_parse__`.
-->
バージョン **v1.0** から、`dataclasses.InitVar` でアノテーションが付与されたフィールドは、
`__post_init__` *と* `__post_init_post_parse__` の両方に渡されます。

```py
{!.tmp_examples/dataclasses_initvars.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
### Difference with stdlib dataclasses
-->

### 標準ライブラリデータクラスとの違い

<!--
Note that the `dataclasses.dataclass` from python stdlib implements only the `__post_init__` method since it doesn't run a validation step.
-->
Python 標準ライブラリの `dataclasses.dataclass` は検証ステップを実行しないため、
`__post_init__` メソッドのみを実装していることに注意してください。

<!--
When substituting usage of `dataclasses.dataclass` with `pydantic.dataclasses.dataclass`, it is recommended to move the code executed in the `__post_init__` method to the `__post_init_post_parse__` method, and only leave behind part of code which needs to be executed before validation.
-->
`dataclasses.dataclass` を `pydantic.dataclasses.dataclass` で代用する場合は、
`__post_init__` メソッドで実行したコードを `__post_init_post_parse__` に移動し、
検証前に実行する必要のあるコードの一部だけを残しておくことをおすすめします。

<!--
## JSON Dumping
-->
## JSON ダンピング

<!--
Pydantic dataclasses do not feature a `.json()` function. To dump them as JSON, you will need to make use of the `pydantic_encoder` as follows:
-->
Pydantic データクラスは `.json()` 関数がありません。JSON としてダンプするには、次のように `pydantic_encoder` を利用する必要があります:

```py
{!.tmp_examples/dataclasses_json_dumps.py!}
```
