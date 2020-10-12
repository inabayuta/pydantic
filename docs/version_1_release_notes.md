<!--
After 2.5 years of development with contributions from over 80 people and 62 releases, *pydantic* has reached
version 1!
-->
2.5 年の開発期間、80 人以上の人々の貢献、62 のリリースを経て、*pydantic* はバージョン1に到達しました!

<!--
While the fundamentals of *pydantic* have remained unchanged since the previous release 
[v0.32.2](changelog.md#v0322-2019-08-17) (indeed, since *pydantic* began in early 2017); 
a number of things have changed which you may wish to be aware of while migrating to Version 1.
-->
*pydantic* の基本は以前のリリース [v0.32.2](changelog.md#v0322-2019-08-17) (2017年の初めに開始されて以来)以降変更されていませんが、
バージョン1に移行する際に注意したいことがいくつかあります。

<!--
Below is a list of significant changes, for a full list of changes see release notes for 
[v1.0b1](changelog.md#v10b1-2019-10-01), [v1.0b2](changelog.md#v10b2-2019-10-07), 
and [v1.0](changelog.md#v10-2019-10-23).
-->
以下は重要な変更のリストです。
変更の完全なリストは [v1.0b1](changelog.md#v10b1-2019-10-01)、[v1.0b2](changelog.md#v10b2-2019-10-07)、
および [v1.0](changelog.md#v10-2019-10-23) を参照してください。

<!--
## What's new in pydantic v1
-->
## Pydantic v1 の新機能

<!--
### Root validators
-->
### ルートバリデータ

<!--
A new decorator [`root_validator`](usage/validators.md#root-validators) has been added to allow validation of entire
models.
-->
モデル全体の検証を可能にするために、新しいデコレータ [`root_validator`](usage/validators.md#root-validators) が追加されました。

<!--
### Custom JSON encoding/decoding
-->
### カスタム JSON エンコード/デコード

<!--
There are new `Config` settings to allow 
[Custom JSON (de)serialisation](usage/exporting_models.md#custom-json-deserialisation). This can allow alternative
JSON implementations to be used with significantly improved performance.
-->
[Custom JSON のシリアライズ・デシリアライズ](usage/exporting_models.md#custom-json-deserialisation)を可能にする新しい `Config` 設定があります。
これにより、代替の JSON 実装を使用して、パフォーマンスを大幅に向上させました。

<!--
### Boolean parsing
-->
### Boolean パース

<!--
The logic for [parsing and validating boolean values](usage/types.md#booleans) has been overhauled to only allow
a defined set of values rather than allowing any value as it used to. 
-->
[Boolean 値のパースとバリデーション](usage/types.md#booleans)が見直され、以前のように値を許可するのではなく、
定義された値のセットのみを許可するようになりました。

<!--
### URL parsing
-->
### URL パース

<!--
The logic for parsing URLs (and related objects like DSNs) has been completely re-written to provide more useful
error messages, greater simplicity and more flexibility.
-->
URL (および DSN などの関連オブジェクト)を解析するためのロジックが完全に書き直され、
より重要なエラーメッセージなど、よりシンプルで柔軟性のあるものが提供されるようになりました。

<!--
### Performance improvements
-->
### パフォーマンス改善

<!--
Some less "clever" error handling and cleanup of how errors are wrapped (together with many other small changes)
has improved the performance of *pydantic* by ~25%, see 
[samuelcolvin/pydantic#819](https://github.com/samuelcolvin/pydantic/pull/819).
-->
あまり賢くないエラー処理と、エラーのラップ方法のクリーンアップ(そして他の多くの小さな変更)により、*pydantic* のパフォーマンスが最大 25% 向上しました。
[samuelcolvin/pydantic#819](https://github.com/samuelcolvin/pydantic/pull/819) を参照してください。

<!--
### ORM mode improvements
-->
### ORM モードの改善

<!--
There are improvements to [`GetterDict`](usage/models.md#orm-mode-aka-arbitrary-class-instances) to make ORM mode
easier to use and work with root validators, see 
[samuelcolvin/pydantic#822](https://github.com/samuelcolvin/pydantic/pull/822).
-->
[`GetterDict`](usage/models.md#orm-mode-aka-arbitrary-class-instances) が改善され、ORM モードが使いやすくなり、
ルートバリデータが上手く機能するようになりました。
[samuelcolvin/pydantic#822](https://github.com/samuelcolvin/pydantic/pull/822) を参照してください。

<!--
### Settings improvements
-->
### 設定の改善

<!--
There are a number of changes to how [`BaseSettings`](usage/settings.md) works:
-->
[`BaseSettings`](usage/settings.md) の動作にいくつかの変更があります:

<!--
* `case_insensitive` has been renamed to `case_sensitive` and the default has changed to `case_sensitive = False`
-->
* `case_insensitive` は `case_sensitive` に名前が変更され、デフォルトは `case_sensitive = False` に変更されました。

<!--
* the default for `env_prefix` has changed to an empty string, i.e. by default there's no prefix for environment
  variable lookups
-->
* `env_prefix` のデフォルトがからの文字列に変更されました。つまり、デフォルトでは環境変数のルックアップのプレフィックスはありません。

<!--
* aliases are no longer used when looking up environment variables, instead there's a new `env` setting for `Field()` or 
  in `Config.fields`.
-->
* 環境変数を検索する際にエイリアスは使用されなくなりました。
  代わりに `Field()` または `Config.fields` に新しい `env` 設定があります。

<!--
### Improvements to field ordering
-->
### フィールドの順序の改善

<!--
There are some subtle changes to the ordering of fields, see [Model field ordering](usage/models.md#field-ordering)
for more details.
-->
フィールドの順序には小さな変更があります。
詳細は[モデルフィールドの順序](usage/models.md#field-ordering)を参照してください。

<!--
### Schema renamed to Field
-->
### Schema から Field へ名称変更

<!--
The function used for providing extra information about fields has been renamed from `Schema` to `Field`. The
new name makes more sense since the method can be used to provide any sort of information and change the behaviour
of the field, as well as add attributes which are used while [generating a model schema](usage/schema.md).
-->
フィールドに関する追加情報を提供するために使用される関数は `Schema` から `Field` に名前が変更されました。
あらゆる種類の情報を提供したり、フィールドの動作を変更したり、[モデルスキーマを生成する](usage/schema.md)際に使用される属性を追加したりできるため、
新しい名称はより意味のあるものになっています。

<!--
### Improved repr methods and devtools integration 
-->
### repr メソッドの改善と開発ツールの統合

<!--
The `__repr__` and `__str__` method of models as well as most other public classes in *pydantic* have been altered
to be consistent and informative. There's also new [integration with python-devtools](usage/devtools.md).
-->
モデルの `__repr__` メソッド と `__str__`、および *pydantic* のほとんどのパブリッククラスは、
一貫性があり情報量のあるものに変更されました。[python-devtools との新しい統合](usage/devtools.md)もあります。

<!--
### Field constraints checks
-->
### フィールド制約チェック

<!--
Constraints added to `Field()` which are not enforced now cause an error when a model is created, see
[Unenforced Field constraints](usage/schema.md#unenforced-field-constraints) for more details and work-arounds.
-->
実行されない制約が `Field()` に追加されている場合、モデルの作成時にエラーが発生するようになりました。
詳細と回避策については、[実行されないフィールドの制約](usage/schema.md#unenforced-field-constraints)を参照してください。
