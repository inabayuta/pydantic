<!--
!!! note
    **Admission:** I (the primary developer of *pydantic*) also develop python-devtools.
-->
!!! note
    **告白:** 私(*pydantic* の主要な開発者)も python-devtools を開発しています。

<!--
[python-devtools](https://python-devtools.helpmanual.io/) (`pip install devtools`) provides a number of tools which
are useful during python development, including `debug()` an alternative to `print()` which formats output in a way
which should be easier to read than `print` as well as giving information about which file/line the print statement 
is on and what value was printed.
-->
[python-devtools](https://python-devtools.helpmanual.io/) (`pip install devtools`)は、
どのファイル/行が出力されたかが `print()` よりも読みやすく整形する `debug()` など、
いくつかの Python 開発に役立つツールを提供します。

<!--
*pydantic* integrates with *devtools* by implementing the `__pretty__` method on most public classes.
-->
*pydantic* は、ほとんどのパブリッククラスに `__pretty__` メソッドを実装することで devtools と統合しています。

<!--
In particular `debug()` is useful when inspecting models:
-->
特に `debug()` は、モデルを検査するときに役立ちます。

```py
{!.tmp_examples/devtools_main.py!}
```

<!--
Will output in your terminal:
-->
ターミナルではこのように出力されます:

{!.tmp_examples/devtools_main.html!}
