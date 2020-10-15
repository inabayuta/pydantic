<!--
*pydantic* models work with [mypy](http://mypy-lang.org/) provided you use the annotation-only version of
required fields:
-->
*pydantic* モデルは必須フィールドのアノテーションのみのバージョンを使用していれば、
[mypy](http://mypy-lang.org/) で動作します。

```py
{!.tmp_examples/mypy_main.py!}
```

<!--
You can run your code through mypy with:
-->
次のコマンドを使用して、mypy を介してコードを実行できます。

```bash
mypy \
  --ignore-missing-imports \
  --follow-imports=skip \
  --strict-optional \
  pydantic_mypy_test.py
```

<!--
If you call mypy on the example code above, you should see mypy detect the attribute access error:
-->
上記のサンプルコードで mypy を呼び出すと、mypy が属性アクセスエラーを検出するのがわかります。

```
13: error: "Model" has no attribute "middle_name"
```

<!--
## Strict Optional
-->
## 厳格なオプション

<!--
For your code to pass with `--strict-optional`, you need to to use `Optional[]` or an alias of `Optional[]`
for all fields with `None` as the default. (This is standard with mypy.)
-->
コードを `--strict-optional` で渡すには、デフォルトで `None` を持つ全てのフィールドに `Optional[]`
または `Optional[]` のエイリアスを使用する必要があります。(これは mypy の標準です。)

<!--
Pydantic provides a few useful optional or union types:
-->
Pydantic はいくつかの便利なオプションや構造体を提供します。

<!--
* `NoneStr` aka. `Optional[str]`
-->
* `NoneStr` 別名: `Optional[str]`

<!--
* `NoneBytes` aka. `Optional[bytes]`
-->
* `NoneBytes` 別名: `Optional[bytes]`

<!--
* `StrBytes` aka. `Union[str, bytes]`
-->
* `StrBytes` 別名: `Union[str, bytes]`

<!--
* `NoneStrBytes` aka. `Optional[StrBytes]`
-->
* `NoneStrBytes` 別名: `Optional[StrBytes]`

<!--
If these aren't sufficient you can of course define your own.
-->
これらが十分でない場合は、もちろん独自に自分で定義することができます。

<!--
## Mypy Plugin
-->
## mypy プラグイン

<!--
Pydantic ships with a mypy plugin that adds a number of important pydantic-specific
features to mypy that improve its ability to type-check your code.
-->
Pydantic には mypy プラグインが付属しており、mypy に多くの重要な pydantic 固有の機能を追加して、
コードのタイプチェック機能を向上させます。

<!--
See the [pydantic mypy plugin docs](../mypy_plugin.md) for more details.
-->
詳細については、[pydantic の mypy プラグインドキュメント](../mypy_plugin.md) を参照してください。

<!--
## Other pydantic interfaces
-->
## その他の pydantic インターフェース

<!--
Pydantic [dataclasses](dataclasses.md) and the [`validate_arguments` decorator](validation_decorator.md)
should also work well with mypy.
-->
Pydantic [データクラス](dataclasses.md)と [`validate_arguments` デコレータ](validation_decorator.md)もうまく機能します。
