<!--
Where possible *pydantic* uses [standard library types](#standard-library-types) to define fields, thus smoothing
the learning curve. For many useful applications, however, no standard library type exists,
so *pydantic* implements [many commonly used types](#pydantic-types).
-->
可能な場合、*pydantic* は[標準ライブラリ](#standard-library-types)の型を使用してフィールドを定義するので、スムーズに学習できます。
しかし、多くの便利なアプリケーションでは標準ライブラリの型が存在しないため、
*pydantic* は一般的に使用される[多くの型](#pydantic-types)を実装しています。

<!--
If no existing type suits your purpose you can also implement your [own pydantic-compatible types](#custom-data-types)
with custom properties and validation.
-->
目的に合った既存の型がない場合は、
カスタムプロパティやバリデーションを使って独自の [pydantic 互換の型](#custom-data-types)を実装することもできます。

<!--
## Standard Library Types
-->
## 標準ライブラリの型

<!--
*pydantic* supports many common types from the python standard library. If you need stricter processing see
[Strict Types](#strict-types); if you need to constrain the values allowed (e.g. to require a positive int) see
[Constrained Types](#constrained-types).
-->
*pydantic* は、Python 標準ライブラリの多くの一般的なタイプをサポートしています。
より厳密な処理が必要な場合は、[厳密な型](#strict-types)を参照してください。
許可される値を制約する必要がある場合(例: 正の整数であることが必須)は、
[制約された型](#constrained-types)を参照してください。

<!--
`bool`
: see [Booleans](#booleans) below for details on how bools are validated and what values are permitted
-->
`bool`
: bool のバリデーションと許可される値の詳細は、[Boolean](#booleans) を参照してください。

<!--
`int`
: *pydantic* uses `int(v)` to coerce types to an `int`;
  see [this](models.md#data-conversion) warning on loss of information during data conversion
-->
`int`
: *pydantic* は `int(v)` を使用して型を `int` に強制変換します。
  データ変換時の情報の損失については[こちら](models.md#data-conversion)の警告を参照してください。

<!--
`float`
: similarly, `float(v)` is used to coerce values to floats
-->
`float`
: 同様に、`float(v)` は値を `float` に強制変換するために使用します。

<!--
`str`
: strings are accepted as-is, `int` `float` and `Decimal` are coerced using `str(v)`, `bytes` and `bytearray` are
  converted using `v.decode()`, enums inheriting from `str` are converted using `v.value`,
  and all other types cause an error
-->
`str`
: 文字列はそのまま受け入れられ、
  `int`、`float`、`Decimal` は `str(v)` を使用して強制変換され、
  `bytes` と `bytearray` は `v.decode()` を使用して変換され、
  `str` から継承する列挙型は `v.value` を使用して変換され、
  他のすべての型はエラーとなります。

<!--
`bytes`
: `bytes` are accepted as-is, `bytearray` is converted using `bytes(v)`, `str` are converted using `v.encode()`,
  and `int`, `float`, and `Decimal` are coerced using `str(v).encode()`
-->
`bytes`
: `bytes` はそのまま受け入れられ、
  `bytearray` は `bytes(v)` を使用して変換され、
  `str` は `v.encode()` を使用して変換され、
  `int`、`float`、`Decimal` は `str(v).encode()` を使用して強制変換されます。

<!--
`list`
: allows `list`, `tuple`, `set`, `frozenset`, or generators and casts to a list;
  see `typing.List` below for sub-type constraints
-->
`list`
: `list`、`tuple`、`set`、`frozenset`、またはジェネレータをリストにキャストします。
  サブタイプの制約については以下の`typing.List` を参照してください。

<!--
`tuple`
: allows `list`, `tuple`, `set`, `frozenset`, or generators and casts to a tuple;
  see `typing.Tuple` below for sub-type constraints
-->
`tuple`
: `list`、`tuple`、`set`、`frozenset`、またはジェネレータをタプルにキャストします。
  サブタイプの制約については以下の `typing.Tuple` を参照してください。

<!--
`dict`
: `dict(v)` is used to attempt to convert a dictionary;
  see `typing.Dict` below for sub-type constraints
-->
`dict`
: `dict(v)` は、辞書の変換を試みるために使用されます。
  サブライプの制約については以下の `typing.Dict` を参照してください。

<!--
`set`
: allows `list`, `tuple`, `set`, `frozenset`, or generators and casts to a set;
  see `typing.Set` below for sub-type constraints
-->
`set`
: `list`、`tuple`、`set`、`frozenset`、またはジェネレータをセットにキャストします。
  サブタイプの制約については以下の `typing.Set` を参照してください。

<!--
`frozenset`
: allows `list`, `tuple`, `set`, `frozenset`, or generators and casts to a frozen set;
  see `typing.FrozenSet` below for sub-type constraints
-->
`frozenset`
: `list`、`tuple`、`set`、`frozenset`、またはジェネレータをフローズンセットにキャストします。
  サブタイプの制約については以下の `typing.FrozenSet` を参照してください。

<!--
`datetime.date`
: see [Datetime Types](#datetime-types) below for more detail on parsing and validation
-->
`datetime.date`
: パースとバリデーションの詳細は、[Datetime 型](#datetime-types)を参照してください。

<!--
`datetime.time`
: see [Datetime Types](#datetime-types) below for more detail on parsing and validation
-->
`datetime.time`
: パースとバリデーションの詳細は、[Datetime 型](#datetime-types)を参照してください。

<!--
`datetime.datetime`
: see [Datetime Types](#datetime-types) below for more detail on parsing and validation
-->
`datetime.datetime`
: パースとバリデーションの詳細は、[Datetime 型](#datetime-types)を参照してください。

<!--
`datetime.timedelta`
: see [Datetime Types](#datetime-types) below for more detail on parsing and validation
-->
`datetime.timedelta`
: パースとバリデーションの詳細は、[Datetime 型](#datetime-types)を参照してください。

<!--
`typing.Any`
: allows any value include `None`, thus an `Any` field is optional
-->
`typing.Any`
: `None` を含む任意の値を許可するので、`Any` フィールドはオプションです。

<!--
`typing.TypeVar`
: constrains the values allowed based on `constraints` or `bound`, see [TypeVar](#typevar)
-->
`typing.TypeVar`
: `constraints` または `bound` に基づいて、許可される値を成約します。[TypeVar](#typevar) を参照してください。

<!--
`typing.Union`
: see [Unions](#unions) below for more detail on parsing and validation
-->
`typing.Union`
: パースとバリデーションの詳細は、[Unions](#unions) を参照してください。

<!--
`typing.Optional`
: `Optional[x]` is simply short hand for `Union[x, None]`;
  see [Unions](#unions) below for more detail on parsing and validation and [Required Fields](models.md#required-fields) for details about required fields that can receive `None` as a value.
-->
`typing.Optional`
: `Optional[x]` は `Union[x, None]` の省略形です。
  パースとバリデーションの詳細は、[Unions](#unions) を参照してください。
  `None` を値として受け取れる必須フィールドの詳細については、
  [必須フィールド](models.md#required-fields)を参照してください。

<!--
`typing.List`
: see [Typing Iterables](#typing-iterables) below for more detail on parsing and validation
-->
`typing.List`
: パースとバリデーションの詳細は、[イテラブル型](#typing-iterables)を参照してください。

<!--
`typing.Tuple`
: see [Typing Iterables](#typing-iterables) below for more detail on parsing and validation
-->
`typing.Tuple`
: パースとバリデーションの詳細は、[イテラブル型](#typing-iterables)を参照してください。

<!--
`typing.Dict`
: see [Typing Iterables](#typing-iterables) below for more detail on parsing and validation
-->
`typing.Dict`
: パースとバリデーションの詳細は、[イテラブル型](#typing-iterables)を参照してください。

<!--
`typing.Set`
: see [Typing Iterables](#typing-iterables) below for more detail on parsing and validation
-->
`typing.Set`
: パースとバリデーションの詳細は、[イテラブル型](#typing-iterables)を参照してください。

<!--
`typing.FrozenSet`
: see [Typing Iterables](#typing-iterables) below for more detail on parsing and validation
-->
`typing.FrozenSet`
: パースとバリデーションの詳細は、[イテラブル型](#typing-iterables)を参照してください。

<!--
`typing.Sequence`
: see [Typing Iterables](#typing-iterables) below for more detail on parsing and validation
-->
`typing.Sequence`
: パースとバリデーションの詳細は、[イテラブル型](#typing-iterables)を参照してください。

<!--
`typing.Iterable`
: this is reserved for iterables that shouldn't be consumed. See [Infinite Generators](#infinite-generators) below for more detail on parsing and validation
-->
`typing.Iterable`
: これは消費されるべきでないイテラブルのために予約されています。
  パースとバリデーションの詳細は、[イテラブルジェネレータ](#infinite-generators)を参照してください。

<!--
`typing.Type`
: see [Type](#type) below for more detail on parsing and validation
-->
`typing.Type`
: パースとバリデーションの詳細は、[Type](#type)を参照してください。

<!--
`typing.Callable`
: see [Callable](#callable) below for more detail on parsing and validation
-->
`typing.Callable`
: パースとバリデーションの詳細は、[Callable](#callable)を参照してください。

<!--
`typing.Pattern`
: will cause the input value to be passed to `re.compile(v)` to create a regex pattern
-->
`typing.Pattern`
: 値を `re.compile(v)` に渡して正規表現パターンを作成します。

<!--
`ipaddress.IPv4Address`
: simply uses the type itself for validation by passing the value to `IPv4Address(v)`;
  see [Pydantic Types](#pydantic-types) for other custom IP address types
-->
`ipaddress.IPv4Address`
: 値を `IPv4Address(v)` に渡すことで、バリデーションのために単に型自体を使用します。
  他の IP アドレス型については、[Pydantic Types](#pydantic-types) を参照してください。

<!--
`ipaddress.IPv4Interface`
: simply uses the type itself for validation by passing the value to `IPv4Address(v)`;
  see [Pydantic Types](#pydantic-types) for other custom IP address types
-->
`ipaddress.IPv4Interface`
: 値を `IPv4Address(v)` に渡すことで、バリデーションのために単に型自体を使用します。
  他の IP アドレス型については、[Pydantic Types](#pydantic-types) を参照してください。

<!--
`ipaddress.IPv4Network`
: simply uses the type itself for validation by passing the value to `IPv4Network(v)`;
  see [Pydantic Types](#pydantic-types) for other custom IP address types
-->
`ipaddress.IPv4Network`
: 値を `IPv4Network(v)` に渡すことで、バリデーションのために単に型自体を使用します。
  他の IP アドレス型については、[Pydantic Types](#pydantic-types) を参照してください。

<!--
`ipaddress.IPv6Address`
: simply uses the type itself for validation by passing the value to `IPv6Address(v)`;
  see [Pydantic Types](#pydantic-types) for other custom IP address types
-->
`ipaddress.IPv6Address`
: 値を `IPv4Address(v)` に渡すことで、バリデーションのために単に型自体を使用します。
  他の IP アドレス型については、[Pydantic Types](#pydantic-types) を参照してください。

<!--
`ipaddress.IPv6Interface`
: simply uses the type itself for validation by passing the value to `IPv6Interface(v)`;
  see [Pydantic Types](#pydantic-types) for other custom IP address types
-->
`ipaddress.IPv6Interface`
: 値を `IPv4Interface(v)` に渡すことで、バリデーションのために単に型自体を使用します。
  他の IP アドレス型については、[Pydantic Types](#pydantic-types) を参照してください。

<!--
`ipaddress.IPv6Network`
: simply uses the type itself for validation by passing the value to `IPv6Network(v)`;
  see [Pydantic Types](#pydantic-types) for other custom IP address types
-->
: 値を `IPv6Network(v)` に渡すことで、バリデーションのために単に型自体を使用します。
  他の IP アドレス型については、[Pydantic Types](#pydantic-types) を参照してください。

<!--
`enum.Enum`
: checks that the value is a valid Enum instance

`subclass of enum.Enum`
: checks that the value is a valid member of the enum;
  see [Enums and Choices](#enums-and-choices) for more details
-->
`enum.Enum`
: 値が列挙型の有効なメンバーであることを確認します。
  詳細については[列挙型と Choices](#enums-and-choices) を参照してください。

<!--
`enum.IntEnum`
: checks that the value is a valid IntEnum instance

`subclass of enum.IntEnum`
: checks that the value is a valid member of the integer enum;
  see [Enums and Choices](#enums-and-choices) for more details
-->
`enum.IntEnum`
: 値が列挙数値型の有効なメンバーであることを確認します。
  詳細については[列挙型と Choices](#enums-and-choices) を参照してください。

<!--
`decimal.Decimal`
: *pydantic* attempts to convert the value to a string, then passes the string to `Decimal(v)`
-->
`decimal.Decimal`
: *pydantic* は値を文字列に変換しようとし、その文字列を `Decimal(v)` に渡します。

<!--
`pathlib.Path`
: simply uses the type itself for validation by passing the value to `Path(v)`;
  see [Pydantic Types](#pydantic-types) for other more strict path types
-->
`pathlib.Path`
: 値を `Path(v)` に渡すことで、単にバリデーションのために型自体を使用します。
  他のより厳密なパス型については、[Pydantic 型](#pydantic-types)を参照してください。

<!--
`uuid.UUID`
: strings and bytes (converted to strings) are passed to `UUID(v)`, with a fallback to `UUID(bytes=v)` for `bytes` and `bytearray`;
  see [Pydantic Types](#pydantic-types) for other stricter UUID types
-->
`uuid.UUID`
: 文字列とバイト(文字列に変換)は `UUID(v)` に渡され、
  `bytes` と `bytearray` は `UUID(bytes=v)` にフォールバックされます。
  他のより厳密な UUID 型については、[Pydantic 型](#pydantic-types)を参照してください。

<!--
`ByteSize`
: converts a bytes string with units to bytes
-->
`ByteSize`
: 単位を含むバイト文字列をバイトに変換します。

<!--
### Typing Iterables
-->
### 反復可能な型

<!--
*pydantic* uses standard library `typing` types as defined in PEP 484 to define complex objects.
-->
*pydantic* は PEP 484 で定義されている標準ライブラリ `typing` の型を使用して、複雑なオブジェクトを定義します。

```py
{!.tmp_examples/types_iterables.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
### Infinite Generators
-->
### 無限ジェネレータ

<!--
If you have a generator you can use `Sequence` as described above. In that case, the
generator will be consumed and stored on the model as a list and its values will be
validated with the sub-type of `Sequence` (e.g. `int` in `Sequence[int]`).
-->
ジェネレータを持っている場合は、上記のように `Sequence` を使用することができます。
その場合、ジェネレータは消費されてリストとしてモデルに格納され、
その値は `Sequence` のサブタイプ(例: `Sequence[int]` の `int`)でバリデーションされます。

<!--
But if you have a generator that you don't want to be consumed, e.g. an infinite
generator or a remote data loader, you can define its type with `Iterable`:
-->
しかし、無限ジェネレータやリモートデータローダなど、消費されたくないジェネレータがある場合は、
`Iterable` でそのタイプを定義することができます:

```py
{!.tmp_examples/types_infinite_generator.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
!!! warning
    `Iterable` fields only perform a simple check that the argument is iterable and
    won't be consumed.

    No validation of their values is performed as it cannot be done without consuming
    the iterable.
-->
!!! warning
    イテレート可能なフィールドは、引数がイテレート可能であり消費されないという単純なチェックを行うだけです。
    
    イテレートを消費しないとバリデーションできないので、値の検証は行われません。

<!--
!!! tip
    If you want to validate the values of an infinite generator you can create a
    separate model and use it while consuming the generator, reporting the validation
    errors as appropriate.

    pydantic can't validate the values automatically for you because it would require
    consuming the infinite generator.
-->
!!! tip
    無限ジェネレーターの値をバリデーションする場合は、別のモデルを作成し、
    ジェネレーターを使用しながらそれを使用して、必要に応じてバリデーションエラーを報告できます。

    pydantic は、無限ジェネレーターを使用する必要があるため、値を自動でバリデーションできません。

<!--
## Validating the first value
-->
## 最初の値の検証

<!--
You can create a [validator](validators.md) to validate the first value in an infinite generator and still not consume it entirely.
-->
[バリデーター](validators.md)を作成して、無限ジェネレーターの最初の値をバリデーションし、
それを完全に消費しないようにすることができます。

```py
{!.tmp_examples/types_infinite_generator_validate_first.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
### Unions
-->
### ユニオン

<!--
The `Union` type allows a model attribute to accept different types, e.g.:
-->
`Union` タイプを使用すると、モデル属性が異なる型を受け入れることができます。例:

<!--
!!! warning
    This script is complete, it should run "as is". However, it may not reflect the desired behavior; see below.
-->
!!! warning
    このスクリプトは完了しています。「そのまま」実行する必要があります。
    ただし、目的の動作を反映していない場合があります。 下記参照。

```py
{!.tmp_examples/types_union_incorrect.py!}
```

<!--
However, as can be seen above, *pydantic* will attempt to 'match' any of the types defined under `Union` and will use
the first one that matches. In the above example the `id` of `user_03` was defined as a `uuid.UUID` class (which
is defined under the attribute's `Union` annotation) but as the `uuid.UUID` can be marshalled into an `int` it
chose to match against the `int` type and disregarded the other types.
-->
ただし、上記のように、*pydantic* は、`Union` で定義されているタイプのいずれかに「一致」しようとし、
一致する最初のタイプを使用します。 上記の例では、`user_03` の `id` は `uuid.UUID` クラス
(属性の `Union` アノテーションで定義されています)として定義されていますが、
`uuid.UUID` は `int` タイプと一致することを選択し、他の型を無視します。

<!--
As such, it is recommended that, when defining `Union` annotations, the most specific type is included first and
followed by less specific types. In the above example, the `UUID` class should precede the `int` and `str`
classes to preclude the unexpected representation as such:
-->
そのため、`Union` アノテーションを定義する際には最も具体的なタイプを最初に含め、
次にあまり具体的でないタイプを含めることをお勧めします。上記の例では、
`UUID` クラスを `int` クラスと `str` クラスの前に置いて、予期しない表現を排除する必要があります。

```py
{!.tmp_examples/types_union_correct.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
!!! tip
    The type `Optional[x]` is a shorthand for `Union[x, None]`.

    `Optional[x]` can also be used to specify a required field that can take `None` as a value.

    See more details in [Required Fields](models.md#required-fields).
-->
!!! tip
    `Optional[x]` は `Union[x, None]` のショートハンドです。
    また、`Optional [x]` を使用して `None` を値とする必須フィールドを指定することもできます。
    
    詳細については、[必須フィールド](models.md＃required-fields)を参照してください。

<!--
### Enums and Choices
-->
### Enum と Choices

<!--
*pydantic* uses python's standard `enum` classes to define choices.
-->
*pydantic* は Python 標準の `enum` クラスを使用して choices を定義します。

```py
{!.tmp_examples/types_choices.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
### Datetime Types
-->
### Datetime 型

<!--
*Pydantic* supports the following [datetime](https://docs.python.org/library/datetime.html#available-types)
types:
-->
*Pydantic* は以下の [datetime](https://docs.python.org/library/datetime.html#available-types) 型をサポートします:

<!--
* `datetime` fields can be:

    * `datetime`, existing `datetime` object
    * `int` or `float`, assumed as Unix time, i.e. seconds (if >= `-2e10` or <= `2e10`) or milliseconds (if < `-2e10`or > `2e10`) since 1 January 1970
    * `str`, following formats work:

        * `YYYY-MM-DD[T]HH:MM[:SS[.ffffff]][Z[±]HH[:]MM]]]`
        * `int` or `float` as a string (assumed as Unix time)
-->
* `datetime` フィールドは次のものがあります:

    * `datetime`: 既存の `datetime` オブジェクト
    * `int` または `float`: Unix 時間とみなされます。つまり1970年1月1日からの秒(if >= `-2e10` or <= `2e10`)またはミリ秒(if < `-2e10`or > `2e10`)
    * `str` は以下のフォーマットで動作します:

        * `YYYY-MM-DD[T]HH:MM[:SS[.ffffff]][Z[±]HH[:]MM]]]`
        * 文字列としての `int` または `float` (Unix時間と想定)

<!--
* `date` fields can be:

    * `date`, existing `date` object
    * `int` or `float`, see `datetime`
    * `str`, following formats work:

        * `YYYY-MM-DD`
        * `int` or `float`, see `datetime`
-->
* `date` フィールドは次のものがあります:

    * `date`: 既存の `date` オブジェクト
    * `int` または `float`: `datetime` を参照
    * `str` は以下のフォーマットで動作します:

        * `YYYY-MM-DD`
        * `int` または `float`: `datetime` を参照

<!--
* `time` fields can be:

    * `time`, existing `time` object
    * `str`, following formats work:

        * `HH:MM[:SS[.ffffff]]`
-->
* `time` フィールドは次のものがあります:

    * `time`: 既存の `time` オブジェクト
    * `str` は以下のフォーマットで動作します:

        * `HH:MM[:SS[.ffffff]]`

<!--
* `timedelta` fields can be:

    * `timedelta`, existing `timedelta` object
    * `int` or `float`, assumed as seconds
    * `str`, following formats work:

        * `[-][DD ][HH:MM]SS[.ffffff]`
        * `[±]P[DD]DT[HH]H[MM]M[SS]S` (ISO 8601 format for timedelta)
-->
* `timedelta` フィールドは次のものがあります:

    * `timedelta`: 既存の `timedelta` オブジェクト
    * `int` または `float`: 秒とみなされます
    * `str` は以下のフォーマットで動作します:

        * `[-][DD ][HH:MM]SS[.ffffff]`
        * `[±]P[DD]DT[HH]H[MM]M[SS]S` (timedelta の ISO 8601 フォーマット)

```py
{!.tmp_examples/types_dt.py!}
```

### Booleans

<!--
!!! warning
    The logic for parsing `bool` fields has changed as of version **v1.0**.

    Prior to **v1.0**, `bool` parsing never failed, leading to some unexpected results.
    The new logic is described below.
-->
!!! warning
    **v1.0** 以前は `bool` のパースが失敗せず、予期しない結果が発生していました。
    以下で新しいロジックについて説明します。

<!--
A standard `bool` field will raise a `ValidationError` if the value is not one of the following:
-->
標準の `bool` フィールドは、値が以下のいずれかでない場合、`ValidationError` を発生させます。

<!--
* A valid boolean (i.e. `True` or `False`),
* The integers `0` or `1`,
* a `str` which when converted to lower case is one of
  `'0', 'off', 'f', 'false', 'n', 'no', '1', 'on', 't', 'true', 'y', 'yes'`
* a `bytes` which is valid (per the previous rule) when decoded to `str`
-->
* 有効な boolean (例: `True` または `False`)
* 数値 `0` または `1`
* 小文字に変換された際に `'0', 'off', 'f', 'false', 'n', 'no', '1', 'on', 't', 'true', 'y', 'yes'` のいずれかである
* `str` にデコードされたときに(前のルールに従って)有効な `bytes`。

<!--
!!! note
    If you want stricter boolean logic (e.g. a field which only permits `True` and `False`) you can
    use [`StrictBool`](#strict-types).
-->
!!! note
    より厳密な boolean ロジック(例えば `True`と `False` のみを許可するフィールド)が必要な場合は、
    [`StrictBool`](#strict-types) を使用できます。

<!--
Here is a script demonstrating some of these behaviors:
-->
以下に、これらの動作のいくつかを示すスクリプトを示します:


```py
{!.tmp_examples/types_boolean.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

### Callable

Fields can also be of type `Callable`:

```py
{!.tmp_examples/types_callable.py!}
```
_(This script is complete, it should run "as is")_

!!! warning
    Callable fields only perform a simple check that the argument is
    callable; no validation of arguments, their types, or the return
    type is performed.

### Type

*pydantic* supports the use of `Type[T]` to specify that a field may only accept classes (not instances)
that are subclasses of `T`.

```py
{!.tmp_examples/types_type.py!}
```
_(This script is complete, it should run "as is")_

You may also use `Type` to specify that any class is allowed.

```py
{!.tmp_examples/types_bare_type.py!}
```
_(This script is complete, it should run "as is")_

### TypeVar

`TypeVar` is supported either unconstrained, constrained or with a bound.

```py
{!.tmp_examples/types_typevar.py!}
```
_(This script is complete, it should run "as is")_

## Literal Type

!!! note
    This is a new feature of the python standard library as of python 3.8;
    prior to python 3.8, it requires the [typing-extensions](https://pypi.org/project/typing-extensions/) package.

*pydantic* supports the use of `typing.Literal` (or `typing_extensions.Literal` prior to python 3.8)
as a lightweight way to specify that a field may accept only specific literal values:

```py
{!.tmp_examples/types_literal1.py!}
```
_(This script is complete, it should run "as is")_

One benefit of this field type is that it can be used to check for equality with one or more specific values
without needing to declare custom validators:

```py
{!.tmp_examples/types_literal2.py!}
```
_(This script is complete, it should run "as is")_

With proper ordering in an annotated `Union`, you can use this to parse types of decreasing specificity:

```py
{!.tmp_examples/types_literal3.py!}
```
_(This script is complete, it should run "as is")_

## Pydantic Types

*pydantic* also provides a variety of other useful types:

`FilePath`
: like `Path`, but the path must exist and be a file

`DirectoryPath`
: like `Path`, but the path must exist and be a directory

`EmailStr`
: requires [email-validator](https://github.com/JoshData/python-email-validator) to be installed;
  the input string must be a valid email address, and the output is a simple string



`NameEmail`
: requires [email-validator](https://github.com/JoshData/python-email-validator) to be installed;
  the input string must be either a valid email address or in the format `Fred Bloggs <fred.bloggs@example.com>`,
  and the output is a `NameEmail` object which has two properties: `name` and `email`.
  For `Fred Bloggs <fred.bloggs@example.com>` the name would be `"Fred Bloggs"`;
  for `fred.bloggs@example.com` it would be `"fred.bloggs"`.


`PyObject`
: expects a string and loads the python object importable at that dotted path;
  e.g. if `'math.cos'` was provided, the resulting field value would be the function `cos`

`Color`
: for parsing HTML and CSS colors; see [Color Type](#color-type)

`Json`
: a special type wrapper which loads JSON before parsing; see [JSON Type](#json-type)

`PaymentCardNumber`
: for parsing and validating payment cards; see [payment cards](#payment-card-numbers)

`AnyUrl`
: any URL; see [URLs](#urls)

`AnyHttpUrl`
: an HTTP URL; see [URLs](#urls)

`HttpUrl`
: a stricter HTTP URL; see [URLs](#urls)

`PostgresDsn`
: a postgres DSN style URL; see [URLs](#urls)

`RedisDsn`
: a redis DSN style URL; see [URLs](#urls)

`stricturl`
: a type method for arbitrary URL constraints; see [URLs](#urls)

`UUID1`
: requires a valid UUID of type 1; see `UUID` [above](#standard-library-types)

`UUID3`
: requires a valid UUID of type 3; see `UUID` [above](#standard-library-types)

`UUID4`
: requires a valid UUID of type 4; see `UUID` [above](#standard-library-types)

`UUID5`
: requires a valid UUID of type 5; see `UUID` [above](#standard-library-types)

`SecretBytes`
: bytes where the value is kept partially secret; see [Secrets](#secret-types)

`SecretStr`
: string where the value is kept partially secret; see [Secrets](#secret-types)

`IPvAnyAddress`
: allows either an `IPv4Address` or an `IPv6Address`

`IPvAnyInterface`
: allows either an `IPv4Interface` or an `IPv6Interface`

`IPvAnyNetwork`
: allows either an `IPv4Network` or an `IPv6Network`

`NegativeFloat`
: allows a float which is negative; uses standard `float` parsing then checks the value is less than 0;
  see [Constrained Types](#constrained-types)

`NegativeInt`
: allows an int which is negative; uses standard `int` parsing then checks the value is less than 0;
  see [Constrained Types](#constrained-types)

`PositiveFloat`
: allows a float which is positive; uses standard `float` parsing then checks the value is greater than 0;
  see [Constrained Types](#constrained-types)

`PositiveInt`
: allows an int which is positive; uses standard `int` parsing then checks the value is greater than 0;
  see [Constrained Types](#constrained-types)

`conbytes`
: type method for constraining bytes;
  see [Constrained Types](#constrained-types)

`condecimal`
: type method for constraining Decimals;
  see [Constrained Types](#constrained-types)

`confloat`
: type method for constraining floats;
  see [Constrained Types](#constrained-types)

`conint`
: type method for constraining ints;
  see [Constrained Types](#constrained-types)

`conlist`
: type method for constraining lists;
  see [Constrained Types](#constrained-types)

`conset`
: type method for constraining sets;
  see [Constrained Types](#constrained-types)

`constr`
: type method for constraining strs;
  see [Constrained Types](#constrained-types)

### URLs

For URI/URL validation the following types are available:

- `AnyUrl`: any scheme allowed, TLD not required
- `AnyHttpUrl`: schema `http` or `https`, TLD not required
- `HttpUrl`: schema `http` or `https`, TLD required, max length 2083
- `PostgresDsn`: schema `postgres` or `postgresql`, user info required, TLD not required
- `RedisDsn`: schema `redis`, user info not required, tld not required (CHANGED: user info not required from
  **v1.6** onwards)
- `stricturl`, method with the following keyword arguments:
    - `strip_whitespace: bool = True`
    - `min_length: int = 1`
    - `max_length: int = 2 ** 16`
    - `tld_required: bool = True`
    - `allowed_schemes: Optional[Set[str]] = None`

The above types (which all inherit from `AnyUrl`) will attempt to give descriptive errors when invalid URLs are
provided:

```py
{!.tmp_examples/types_urls.py!}
```
_(This script is complete, it should run "as is")_

If you require a custom URI/URL type, it can be created in a similar way to the types defined above.

#### URL Properties

Assuming an input URL of `http://samuel:pass@example.com:8000/the/path/?query=here#fragment=is;this=bit`,
the above types export the following properties:

- `scheme`: always set - the url schema (`http` above)
- `host`: always set - the url host (`example.com` above)
- `host_type`: always set - describes the type of host, either:

  - `domain`: e.g. `example.com`,
  - `int_domain`: international domain, see [below](#international-domains), e.g. `exampl£e.org`,
  - `ipv4`: an IP V4 address, e.g. `127.0.0.1`, or
  - `ipv6`: an IP V6 address, e.g. `2001:db8:ff00:42`

- `user`: optional - the username if included (`samuel` above)
- `password`: optional - the password if included (`pass` above)
- `tld`: optional - the top level domain (`com` above),
  **Note: this will be wrong for any two-level domain, e.g. "co.uk".** You'll need to implement your own list of TLDs
  if you require full TLD validation
- `port`: optional - the port (`8000` above)
- `path`: optional - the path (`/the/path/` above)
- `query`: optional - the URL query (aka GET arguments or "search string") (`query=here` above)
- `fragment`: optional - the fragment (`fragment=is;this=bit` above)

If further validation is required, these properties can be used by validators to enforce specific behaviour:

```py
{!.tmp_examples/types_url_properties.py!}
```
_(This script is complete, it should run "as is")_

#### International Domains

"International domains" (e.g. a URL where the host or TLD includes non-ascii characters) will be encoded via
[punycode](https://en.wikipedia.org/wiki/Punycode) (see
[this article](https://www.xudongz.com/blog/2017/idn-phishing/) for a good description of why this is important):

```py
{!.tmp_examples/types_url_punycode.py!}
```
_(This script is complete, it should run "as is")_


!!! warning
    #### Underscores in Hostnames

    In *pydantic* underscores are allowed in all parts of a domain except the tld.
    Technically this might be wrong - in theory the hostname cannot have underscores, but subdomains can.

    To explain this; consider the following two cases:

    - `exam_ple.co.uk`: the hostname is `exam_ple`, which should not be allowed since it contains an underscore
    - `foo_bar.example.com` the hostname is `example`, which should be allowed since the underscore is in the subdomain

    Without having an exhaustive list of TLDs, it would be impossible to differentiate between these two. Therefore
    underscores are allowed, but you can always do further validation in a validator if desired.

    Also, Chrome, Firefox, and Safari all currently accept `http://exam_ple.com` as a URL, so we're in good
    (or at least big) company.

### Color Type

You can use the `Color` data type for storing colors as per
[CSS3 specification](http://www.w3.org/TR/css3-color/#svg-color). Colors can be defined via:

- [name](http://www.w3.org/TR/SVG11/types.html#ColorKeywords) (e.g. `"Black"`, `"azure"`)
- [hexadecimal value](https://en.wikipedia.org/wiki/Web_colors#Hex_triplet)
  (e.g. `"0x000"`, `"#FFFFFF"`, `"7fffd4"`)
- RGB/RGBA tuples (e.g. `(255, 255, 255)`, `(255, 255, 255, 0.5)`)
- [RGB/RGBA strings](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#RGB_colors)
  (e.g. `"rgb(255, 255, 255)"`, `"rgba(255, 255, 255, 0.5)"`)
- [HSL strings](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#HSL_colors)
  (e.g. `"hsl(270, 60%, 70%)"`, `"hsl(270, 60%, 70%, .5)"`)

```py
{!.tmp_examples/types_color.py!}
```
_(This script is complete, it should run "as is")_

`Color` has the following methods:

**`original`**
: the original string or tuple passed to `Color`

**`as_named`**
: returns a named CSS3 color; fails if the alpha channel is set or no such color exists unless
  `fallback=True` is supplied, in which case it falls back to `as_hex`

**`as_hex`**
: returns a string in the format `#fff` or `#ffffff`; will contain 4 (or 8) hex values if the alpha channel is set,
  e.g. `#7f33cc26`

**`as_rgb`**
: returns a string in the format `rgb(<red>, <green>, <blue>)`, or `rgba(<red>, <green>, <blue>, <alpha>)`
  if the alpha channel is set

**`as_rgb_tuple`**
: returns a 3- or 4-tuple in RGB(a) format. The `alpha` keyword argument can be used to define whether
  the alpha channel should be included;
  options: `True` - always include, `False` - never include, `None` (default) - include if set

**`as_hsl`**
: string in the format `hsl(<hue deg>, <saturation %>, <lightness %>)`
  or `hsl(<hue deg>, <saturation %>, <lightness %>, <alpha>)` if the alpha channel is set

**`as_hsl_tuple`**
: returns a 3- or 4-tuple in HSL(a) format. The `alpha` keyword argument can be used to define whether
  the alpha channel should be included;
  options: `True` - always include, `False` - never include, `None` (the default)  - include if set

The `__str__` method for `Color` returns `self.as_named(fallback=True)`.

!!! note
    the `as_hsl*` refer to hue, saturation, lightness "HSL" as used in html and most of the world, **not**
    "HLS" as used in python's `colorsys`.

### Secret Types

You can use the `SecretStr` and the `SecretBytes` data types for storing sensitive information
that you do not want to be visible in logging or tracebacks.
`SecretStr` and `SecretBytes` can be initialized idempotently or by using `str` or `bytes` literals respectively.
The `SecretStr` and `SecretBytes` will be formatted as either `'**********'` or `''` on conversion to json.

```py
{!.tmp_examples/types_secret_types.py!}
```
_(This script is complete, it should run "as is")_

### Json Type

You can use `Json` data type to make *pydantic* first load a raw JSON string.
It can also optionally be used to parse the loaded object into another type base on
the type `Json` is parameterised with:

```py
{!.tmp_examples/types_json_type.py!}
```
_(This script is complete, it should run "as is")_

### Payment Card Numbers

The `PaymentCardNumber` type validates [payment cards](https://en.wikipedia.org/wiki/Payment_card)
(such as a debit or credit card).

```py
{!.tmp_examples/types_payment_card_number.py!}
```
_(This script is complete, it should run "as is")_

`PaymentCardBrand` can be one of the following based on the BIN:

* `PaymentCardBrand.amex`
* `PaymentCardBrand.mastercard`
* `PaymentCardBrand.visa`
* `PaymentCardBrand.other`

The actual validation verifies the card number is:

* a `str` of only digits
* [luhn](https://en.wikipedia.org/wiki/Luhn_algorithm) valid
* the correct length based on the BIN, if Amex, Mastercard or Visa, and between
  12 and 19 digits for all other brands

## Constrained Types

The value of numerous common types can be restricted using `con*` type functions:

```py
{!.tmp_examples/types_constrained.py!}
```
_(This script is complete, it should run "as is")_

Where `Field` refers to the [field function](schema.md#field-customisation).

## Strict Types

You can use the `StrictStr`, `StrictInt`, `StrictFloat`, and `StrictBool` types
to prevent coercion from compatible types.
These types will only pass validation when the validated value is of the respective type or is a subtype of that type.
This behavior is also exposed via the `strict` field of the `ConstrainedStr`, `ConstrainedFloat` and
`ConstrainedInt` classes and can be combined with a multitude of complex validation rules.

The following caveats apply:

- `StrictInt` (and the `strict` option of `ConstrainedInt`) will not accept `bool` types,
    even though `bool` is a subclass of `int` in Python. Other subclasses will work.
- `StrictFloat` (and the `strict` option of `ConstrainedFloat`) will not accept `int`.

```py
{!.tmp_examples/types_strict.py!}
```
_(This script is complete, it should run "as is")_

## ByteSize

You can use the `ByteSize` data type to convert byte string representation to
raw bytes and print out human readable versions of the bytes as well.

!!! info
    Note that `1b` will be parsed as "1 byte" and not "1 bit".

```py
{!.tmp_examples/types_bytesize.py!}
```
_(This script is complete, it should run "as is")_

## Custom Data Types

You can also define your own custom data types. There are several ways to achieve it.

### Classes with `__get_validators__`

You use a custom class with a classmethod `__get_validators__`. It will be called
to get validators to parse and validate the input data.

!!! tip
    These validators have the same semantics as in [Validators](validators.md), you can
    declare a parameter `config`, `field`, etc.

```py
{!.tmp_examples/types_custom_type.py!}
```
_(This script is complete, it should run "as is")_

Similar validation could be achieved using [`constr(regex=...)`](#constrained-types) except the value won't be
formatted with a space, the schema would just include the full pattern and the returned value would be a vanilla string.

See [Schema](schema.md) for more details on how the model's schema is generated.

### Arbitrary Types Allowed

You can allow arbitrary types using the `arbitrary_types_allowed` config in the
[Model Config](model_config.md).

```py
{!.tmp_examples/types_arbitrary_allowed.py!}
```
_(This script is complete, it should run "as is")_

### Generic Classes as Types

!!! warning
    This is an advanced technique that you might not need in the beginning. In most of
    the cases you will probably be fine with standard *pydantic* models.

You can use
[Generic Classes](https://docs.python.org/3/library/typing.html#typing.Generic) as
field types and perform custom validation based on the "type parameters" (or sub-types)
with `__get_validators__`.

If the Generic class that you are using as a sub-type has a classmethod
`__get_validators__` you don't need to use `arbitrary_types_allowed` for it to work.

Because you can declare validators that receive the current `field`, you can extract
the `sub_fields` (from the generic class type parameters) and validate data with them.

```py
{!.tmp_examples/types_generics.py!}
```
_(This script is complete, it should run "as is")_
