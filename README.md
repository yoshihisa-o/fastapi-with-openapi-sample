# OpenAPI YAML
- 以下のサンプルを利用する。
- https://github.com/OAI/OpenAPI-Specification/blob/main/examples/v3.0/petstore-expanded.yaml

# schema validation 失敗時のレスポンスをカスタマイズする方法
- https://fastapi.tiangolo.com/tutorial/handling-errors/
- https://github.com/tiangolo/fastapi/issues/484
- https://stackoverflow.com/questions/60274141/is-it-possible-to-change-the-pydantic-error-messages-in-fastapi

# 環境構築
- ASGI web server であるuviconrn をインストールする。
```
pip install uvicorn
```

# OpenAPI からFastAPI app を生成する
- [GitHub - koxudaxi/fastapi-code-generator: This code generator creates FastAPI app from an openapi file.](https://github.com/koxudaxi/fastapi-code-generator)
```
pip install fastapi-code-generator
# app の下にmodels.py とmain.py が出力される
fastapi-codegen --input app/openapi/petstore-expanded.yaml --output app
```

# API 起動
```
cd app/
uvicorn main:app --host 0.0.0.0 --port 80 --reload
```

# 動作確認
```
# GET /pets
curl -X GET http://127.0.0.1/pets

# POST /pets
## name が必須。
curl -X POST http://127.0.0.1/pets \
   -H 'Content-Type: application/json' \
   -d '{"name":"Max"}'

## name なし
curl -X POST http://127.0.0.1/pets \
   -H 'Content-Type: application/json' \
   -d '{}'
> デフォルトでは 422 {"detail":[{"loc":["body","name"],"msg":"field required","type":"value_error.missing"}]}%  

## 未知のプロパティあり(foo) 
curl -X POST http://127.0.0.1/pets \
   -H 'Content-Type: application/json' \
   -d '{"name":"Max", "foo": "bar"}'
> 200
```