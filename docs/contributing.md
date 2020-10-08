<!--
We'd love you to contribute to *pydantic*!
-->
*pydantic* に貢献して下さい!

<!--
## Issues
-->
## イシュー

<!--
Questions, feature requests and bug reports are all welcome as issues. **However, to report a security
vulnerability, please see our [security policy](https://github.com/samuelcolvin/pydantic/security/policy).**
-->
質問、機能リクエストおよびバグレポートは全てイシューとして歓迎します。**しかし、セキュリティーの脆弱性を報告する場合は、
[セキュリティーポリシー](https://github.com/samuelcolvin/pydantic/security/policy)を見て下さい。**

<!--
To make it as simple as possible for us to help you, please include the output of the following call in your issue:
-->
できるだけ簡単にサポートできるように、以下の呼び出しの出力をあなたのイシューに含めて下さい。

```bash
python -c "import pydantic.utils; print(pydantic.utils.version_info())"
```
<!--
If you're using *pydantic* prior to **v1.3** (when `version_info()` was added), please manually include OS, python
version and pydantic version.
-->
もし v1.3 ( `version_info()` が追加された版)以前の *pydantic* を使用している場合は、
OS、Python のバージョン及び pydantic のバージョンを含めて下さい。

<!--
Please try to always include the above unless you're unable to install *pydantic* or **know** it's not relevant
to your question or feature request.
-->
*pydantic* がインストールできない場合や、質問や機能リクエストに関係が無いと**知っている**場合を除き、上記を常に含めるようにして下さい。

<!--
## Pull Requests
-->
## プルリクエスト

<!--
It should be extremely simple to get started and create a Pull Request.
*pydantic* is released regularly so you should see your improvements release in a matter of days or weeks.
-->
プルリクエストを作成するのは非常に簡単です。*pydantic* は定期的にリリースされるため、数日から数週間で改善がリリースされるはずです。

<!--
!!! note
    Unless your change is trivial (typo, docs tweak etc.), please create an issue to discuss the change before
    creating a pull request.
-->
!!! note
    あなたの変更が些細なもの(タイプミス、ドキュメントの微調整など)でない限り、プルリクエストを作成する前に、
    変更を議論するためのイシューを作成して下さい。

<!--
If you're looking for something to get your teeth into, check out the
["help wanted"](https://github.com/samuelcolvin/pydantic/issues?q=is%3Aopen+is%3Aissue+label%3A%22help+wanted%22)
label on github.
-->
もし歯が立たないほど難しい何かを探していたら、Github の "help wanted" ラベルを確認して下さい。

<!--
To make contributing as easy and fast as possible, you'll want to run tests and linting locally. Luckily,
*pydantic* has few dependencies, doesn't require compiling and tests don't need access to databases, etc.
Because of this, setting up and running the tests should be very simple.
-->
貢献をできるだけ簡単かつ迅速に行うため、あなたはテストとリンティングをローカルで実行したくなるでしょう。
幸いなことに、 *pydantic* は依存関係がほとんどなく、コンパイル不要で、データベースへのアクセスが必要なテスト等もありません。
このため、テストの設定と実行は非常に簡単です。

<!--
You'll need to have **python 3.6**, **3.7**, or **3.8**, **virtualenv**, **git**, and **make** installed.
-->
**python 3.6**、**3.7**、または **3.8**、**virtualenv**、**git**、および **make** をインストールする必要があります。

<!--
```bash
# 1. clone your fork and cd into the repo directory
git clone git@github.com:<your username>/pydantic.git
cd pydantic

# 2. Set up a virtualenv for running tests
virtualenv -p `which python3.7` env
source env/bin/activate
# (or however you prefer to setup a python environment, 3.6 will work too)

# 3. Install pydantic, dependencies and test dependencies
make install

# 4. Checkout a new branch and make your changes
git checkout -b my-new-feature-branch
# make your changes...

# 5. Fix formatting and imports
make format
# Pydantic uses black to enforce formatting and isort to fix imports
# (https://github.com/ambv/black, https://github.com/timothycrosley/isort)

# 6. Run tests and linting
make
# there are a few sub-commands in Makefile like `test`, `testcov` and `lint`
# which you might want to use, but generally just `make` should be all you need

# 7. Build documentation
make docs
# if you have changed the documentation make sure it builds successfully
# you can also use `make docs-serve` to serve the documentation at localhost:8000

# ... commit, push, and create your pull request
```
-->
```bash
# 1. fork を複製し、レポジトリのディレクトリへ移動
git clone git@github.com:<your username>/pydantic.git
cd pydantic

# 2. テストを実行するため virtualenv を設定
virtualenv -p `which python3.7` env
source env/bin/activate
# (または Python 環境を設定したい場合は、3.6でも動作します)

# 3. pydantic とその依存関係をインストールし、依存性テストを行う
make install

# 4. 新しいブランチをチェックアウトし、変更を加える
git checkout -b my-new-feature-branch
# 変更を加える...

# 5. フォーマットの修正とインポート
make format
# pydantic は black を使用してフォーマットを適用し、isort を使用してインポートを修正します
# (https://github.com/ambv/black, https://github.com/timothycrosley/isort)

# 6. テストとリンティングの実行
make
# Makefile には `test`、`testcov`、`lint` などサブコマンドがいくつかありますが、通常は `make` だけで十分です。

# 7. ドキュメントをビルド
make docs
# ドキュメントを変更した場合は、正常にビルドされることを確認して下さい。
# `make docs-serve` を使用して、localhost:8000 でドキュメントを提供することもできます。

# ... コミット、プッシュ、そしてプルリクエストの作成
```

<!--
**tl;dr**: use `make format` to fix formatting, `make` to run tests and linting & `make docs`
to build the docs.
-->
**要約すると**: `make format` を使用してフォーマットを修正し、`make` でテストとリンティングを実行し、`make docs` でドキュメントをビルドします。
