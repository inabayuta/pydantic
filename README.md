# pydantic

[![CI](https://github.com/samuelcolvin/pydantic/workflows/CI/badge.svg?event=push)](https://github.com/samuelcolvin/pydantic/actions?query=event%3Apush+branch%3Amaster+workflow%3ACI)
[![Coverage](https://codecov.io/gh/samuelcolvin/pydantic/branch/master/graph/badge.svg)](https://codecov.io/gh/samuelcolvin/pydantic)
[![pypi](https://img.shields.io/pypi/v/pydantic.svg)](https://pypi.python.org/pypi/pydantic)
[![CondaForge](https://img.shields.io/conda/v/conda-forge/pydantic.svg)](https://anaconda.org/conda-forge/pydantic)
[![downloads](https://img.shields.io/pypi/dm/pydantic.svg)](https://pypistats.org/packages/pydantic)
[![versions](https://img.shields.io/pypi/pyversions/pydantic.svg)](https://github.com/samuelcolvin/pydantic)
[![license](https://img.shields.io/github/license/samuelcolvin/pydantic.svg)](https://github.com/samuelcolvin/pydantic/blob/master/LICENSE)

<!--
Data validation and settings management using Python type hinting.
-->
Python の型ヒントを使用したデータバリデーションと設定管理

<!--
Fast and extensible, *pydantic* plays nicely with your linters/IDE/brain.
Define how data should be in pure, canonical Python 3.6+; validate it with *pydantic*.
-->
高速で拡張可能な *pydantic* はあなたのリンター/IDE/脳でうまく連携します。

<!--
## Help
-->
## ヘルプ

<!--
See [documentation](https://pydantic-docs.helpmanual.io/) for more details.
-->
より詳細な情報は[ドキュメンテーション](https://pydantic-docs.helpmanual.io/)を見てください。

<!--
## Installation
-->
## インストール

<!--
Install using `pip install -U pydantic` or `conda install pydantic -c conda-forge`.
For more installation options to make *pydantic* even faster,
see the [Install](https://pydantic-docs.helpmanual.io/install/) section in the documentation.
-->
インストールは `pip install -U pydantic` または `conda install pydantic -c conda-forge` を使用します。
*pydantic* をより高速化するインストールオプションの詳細は、ドキュメントの[インストール](https://pydantic-docs.helpmanual.io/install/)を見てください。

<!--
## A Simple Example
-->
## 簡単な例

```py
from datetime import datetime
from typing import List, Optional
from pydantic import BaseModel

class User(BaseModel):
    id: int
    name = 'John Doe'
    signup_ts: Optional[datetime] = None
    friends: List[int] = []

external_data = {'id': '123', 'signup_ts': '2017-06-01 12:22', 'friends': [1, '2', b'3']}
user = User(**external_data)
print(user)
#> User id=123 name='John Doe' signup_ts=datetime.datetime(2017, 6, 1, 12, 22) friends=[1, 2, 3]
print(user.id)
#> 123
```

<!--
## Contributing
-->
## 貢献

<!--
For guidance on setting up a development environment and how to make a
contribution to *pydantic*, see
[Contributing to Pydantic](https://pydantic-docs.helpmanual.io/contributing/).
-->
開発環境の設定と *pydantic* への貢献方法に関するガイダンスは、[Pydantic への貢献](https://pydantic-docs.helpmanual.io/contributing/)を参照してください。

<!--
## Reporting a Security Vulnerability
-->
## セキュリティーの脆弱性の報告

<!--
See our [security policy](https://github.com/samuelcolvin/pydantic/security/policy).
-->
[セキュリティーポリシー](https://github.com/samuelcolvin/pydantic/security/policy)を見てください。
