<!--
Installation is as simple as:
-->
インストールは次のように簡単です:

```py
pip install pydantic
```

<!--
*pydantic* has no required dependencies except python 3.6, 3.7, or 3.8 (and the dataclasses package in python 3.6).
If you've got python 3.6+ and `pip` installed, you're good to go.
-->
*pydantic* は Python 3.6、3.7、3.8 (および Python 3.6 の dataclasses パッケージ)以外に必要な依存関係はありません。

<!--
Pydantic is also available on [conda](https://www.anaconda.com) under the [conda-forge](https://conda-forge.org)
channel:
-->
Pydantic は [conda](https://www.anaconda.com) の [conda-forge](https://conda-forge.org) でも利用可能です。

```bash
conda install pydantic -c conda-forge
```
<!--
*pydantic* can optionally be compiled with [cython](https://cython.org/) which should give a 30-50% performance
improvement. `manylinux` binaries exist for python 3.6, 3.7, and 3.8, so if you're installing from PyPI on linux, you
should get a compiled version of *pydantic* with no extra work. If you're installing manually, install `cython`
before installing *pydantic* and compilation should happen automatically. Compilation with cython
[is not tested](https://github.com/samuelcolvin/pydantic/issues/555) on windows or mac.
-->
*pydantic* はオプションで [cython](https://cython.org/) でコンパイルすることができ、30～50%のパフォーマンス向上が期待できます。
Linux 上の PyPI からインストールする場合は、追加作業なしにコンパイル済みの *pydantic* を入手することができます。
手動でインストールする場合は、*pydantic* をインストールする前に `cython` をインストールすると自動的にコンパイルが行われます。
`cython` を使用したコンパイルは Windows や Mac では[テストされていません](https://github.com/samuelcolvin/pydantic/issues/555)。

<!--
To test if *pydantic* is compiled run:
-->
*pydantic* がコンパイルされているテストします:

```py
import pydantic
print('compiled:', pydantic.compiled)
```

<!--
*pydantic* has three optional dependencies:
-->
*pydantic* は3つの依存関係オプションがあります:

<!--
* If you require email validation you can add [email-validator](https://github.com/JoshData/python-email-validator)
-->
* メールの検証が必要な場合は、[email-validator](https://github.com/JoshData/python-email-validator) を追加できます。

<!--
* use of `Literal` prior to python 3.8 relies on [typing-extensions](https://pypi.org/project/typing-extensions/)
-->
* Python 3.8 以前の `Literal` の使用は、[typing-extensions](https://pypi.org/project/typing-extensions/) に依存しています。

<!--
* [dotenv file support](usage/settings.md#dotenv-env-support) with `Settings` requires
  [python-dotenv](https://pypi.org/project/python-dotenv)
-->
* [dotenv ファイルのサポート](usage/settings.md#dotenv-env-support)の `Settings` は [python-dotenv](https://pypi.org/project/python-dotenv) が必要です。

<!--
To install these along with *pydantic*:
-->
これらを *pydantic* と一緒にインストールするには:

```bash
pip install pydantic[email]
# or
pip install pydantic[typing_extensions]
# or
pip install pydantic[dotenv]
# or just
pip install pydantic[email,typing_extensions,dotenv]
```

<!--
Of course, you can also install these requirements manually with `pip install email-validator` and/or `pip install typing_extensions`.
-->
もちろん、`pip install email-validator` や `pip install typing_extensions` を使って手動でインストールすることもできます。

<!--
And if you prefer to install *pydantic* directly from the repository:
-->
そして、リポジトリから直接*pydantic*をインストールしたい場合は:

```bash
pip install git+git://github.com/samuelcolvin/pydantic@master#egg=pydantic
# or with extras
pip install git+git://github.com/samuelcolvin/pydantic@master#egg=pydantic[email,typing_extensions]
```
