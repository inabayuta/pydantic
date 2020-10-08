<!--
[datamodel-code-generator](https://github.com/koxudaxi/datamodel-code-generator/) is a command to generate pydantic models from other data types.
-->
[datamodel-code-generator](https://github.com/koxudaxi/datamodel-code-generator/) は、他のデータ型から pydantic モデルを作成するコマンドです。

<!--
* Supported source types
    * OpenAPI 3 (YAML/JSON)
    * JSON Schema
    * JSON/YAML Data (It will be converted to JSON Schema)
-->
* サポートされているソースタイプ
    * OpenAPI 3 (YAML/JSON)
    * JSON スキーマ
    * JSON/YAML データ (JSON スキーマに変換されます)

<!--
## Install
-->
## インストール
```bash
pip install datamodel-code-generator
```

<!--
## Example
-->
## 例
<!--
In this case, The datamodel-code-generator creates pydantic models from JSON Schema.
-->
この場合、datamodel-code-generator は JSON スキーマから pydantic モデルを作成します。
```bash
datamodel-codegen  --input person.json --input-file-type jsonschema --output model.py
```

person.json:
```json
{
  "$id": "person.json",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Person",
  "type": "object",
  "properties": {
    "first_name": {
      "type": "string",
      "description": "The person's first name."
    },
    "last_name": {
      "type": "string",
      "description": "The person's last name."
    },
    "age": {
      "description": "Age in years.",
      "type": "integer",
      "minimum": 0
    },
    "pets": {
      "type": "array",
      "items": [
        {
          "$ref": "#/definitions/Pet"
        }
      ]
    },
    "comment": {
      "type": "null"
    }
  },
  "required": [
      "first_name",
      "last_name"
  ],
  "definitions": {
    "Pet": {
      "properties": {
        "name": {
          "type": "string"
        },
        "age": {
          "type": "integer"
        }
      }
    }
  }
}
```

model.py:
```py
{!.tmp_examples/generate_models_person_model.py!}
```

<!--
More information can be found on the
[official documentation](https://koxudaxi.github.io/datamodel-code-generator/)
-->
詳細は[公式ドキュメント](https://koxudaxi.github.io/datamodel-code-generator/)を参照してください。
