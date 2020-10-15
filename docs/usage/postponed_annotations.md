<!--
!!! note
    Both postponed annotations via the future import and `ForwardRef` require python 3.7+.
-->
!!! note
    将来のインポートと `ForwardRef` を介して延期されたアノテーションには、Python3.7 以降が必要です。

<!--
Postponed annotations (as described in [PEP563](https://www.python.org/dev/peps/pep-0563/))
"just work".
-->
延期されたアノテーション([PEP563](https://www.python.org/dev/peps/pep-0563/) で説明されています)は「正常に機能」します。

```py
{!.tmp_examples/postponed_annotations_main.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
Internally, *pydantic*  will call a method similar to `typing.get_type_hints` to resolve annotations.
-->
内部的には、*pydantic* は `typeing.get_type_hints` と同様のメソッドを呼び出してアノテーションを解決します。

<!--
In cases where the referenced type is not yet defined, `ForwardRef` can be used (although referencing the
type directly or by its string is a simpler solution in the case of
[self-referencing models](#self-referencing-models)).
-->
参照される型がまだ定義されていない場合は、ForwardRefを使用できます(ただし、[自己参照モデル](#self-referencing-models)の場合は、
型を直接またはその文字列で参照する方が簡単です)。

<!--
In some cases, a `ForwardRef` won't be able to be resolved during model creation.
For example, this happens whenever a model references itself as a field type.
When this happens, you'll need to call `update_forward_refs` after the model has been created before it can be used:
-->
場合によっては、モデルの作成中に `ForwardRef` を解決できないことがあります。
例えば、モデルがそれ自体をフィールドタイプとして参照するたびに発生します。
これが発生した場合、モデルを作成した後、使用する前に `update_forward_refs` を呼び出す必要があります。

```py
{!.tmp_examples/postponed_annotations_forward_ref.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
!!! warning
    To resolve strings (type names) into annotations (types), *pydantic* needs a namespace dict in which to
    perform the lookup. For this it uses `module.__dict__`, just like `get_type_hints`.
    This means *pydantic* may not play well with types not defined in the global scope of a module.
-->

!!! warning
    文字列(型名)をアノテーション(型)に解決するのに、*pydantic* はルックアップを実行するための名前空間辞書を必要とします。
    このため `get_type_hints` と同様に、`module.__ dict__` を使用します。
    つまり、*pydantic* モジュールのグローバルスコープで定義されていない型ではうまく機能しない可能性があります。

<!--
For example, this works fine:
-->
例えば、以下は正常に動作します:

```py
{!.tmp_examples/postponed_annotations_works.py!}
```

<!--
While this will break:
-->
以下は壊れます:

```py
{!.tmp_examples/postponed_annotations_broken.py!}
```

<!--
Resolving this is beyond the call for *pydantic*: either remove the future import or declare the types globally.
-->
これを解決することは、*pydantic* の必要性を超えています。
将来のインポートを削除するか、型をグローバルに宣言する必要があります。

<!--
## Self-referencing Models
-->
## 自己参照モデル

<!--
Data structures with self-referencing models are also supported, provided the function
`update_forward_refs()` is called once the model is created (you will be reminded
with a friendly error message if you forget).
-->
モデルの作成後に関数 `update_forward_refs()` が呼び出される場合は、
自己参照モデルを使用したデータ構造もサポートされます(呼び出しを忘れると、わかりやすいエラーメッセージが表示されます)。

<!--
Within the model, you can refer to the not-yet-constructed model using a string:
-->
モデル内では、文字列を使って未構築のモデルを参照することができます。

```py
{!.tmp_examples/postponed_annotations_self_referencing_string.py!}
```
<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_

<!--
Since `python 3.7`, you can also refer it by its type, provided you import `annotations` (see
[above](postponed_annotations.md) for support depending on Python
and *pydantic* versions).
-->
Python 3.7 以降、`annotations` をインポートすれば、型で参照することもできます
(Python と *pydantic* のバージョンに応じたサポートについては[上記](postponed_annotations.md)を参照してください)。

```py
{!.tmp_examples/postponed_annotations_self_referencing_annotations.py!}
```

<!--
_(This script is complete, it should run "as is")_
-->
_(このスクリプトは完成しています。「そのまま」実行する必要があります)_
