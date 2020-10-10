<!--
While pydantic will work well with any IDE out of the box, a 
[PyCharm plugin](https://plugins.jetbrains.com/plugin/12861-pydantic)
offering improved pydantic integration is available on the JetBrains Plugins Repository for PyCharm.
You can install the plugin for free from the plugin marketplace
(PyCharm's Preferences -> Plugin -> Marketplace -> search "pydantic").
-->
pydantic はどのIDEでも問題なく動作しますが、[PyCharm plugin](https://plugins.jetbrains.com/plugin/12861-pydantic)
はPyCharm用の JetBrains プラグインリポジトリでpydanticの統合を改善しています。
プラグインのマーケットプレイスからインストールできます(PyCharm's Preferences -> Plugin -> Marketplace -> "pydantic" で検索)。

<!--
The plugin currently supports the following features:
-->
プラグインは現在次の機能をサポートしています:

<!--
* For `pydantic.BaseModel.__init__`:
    * Inspection
    * Autocompletion
    * Type-checking
-->
* `pydantic.BaseModel.__init__` 用:
    * 検査
    * オートコンプリート
    * 型チェック

<!--
* For fields of `pydantic.BaseModel`:
    * Refactor-renaming fields updates `__init__` calls, and affects sub- and super-classes
    * Refactor-renaming `__init__` keyword arguments updates field names, and affects sub- and super-classes
-->
* `pydantic.BaseModel` のフィールド用:
    * リファクタ・リネーミングフィールドは `__init__` を更新し、サブクラスおよびスーパークラスに影響を与えます。
    * リファクタ・リネーミングフィールドの `__init__` キーワード引数はフィールド名を更新し、サブクラスおよびスーパークラスに影響を与えます。

<!--
More information can be found on the
[official plugin page](https://plugins.jetbrains.com/plugin/12861-pydantic)
and [Github repository](https://github.com/koxudaxi/pydantic-pycharm-plugin).
-->
より詳細な情報は[公式プラグインページ](https://plugins.jetbrains.com/plugin/12861-pydantic)
および [Github リポジトリ](https://github.com/koxudaxi/pydantic-pycharm-plugin) で確認できます。

