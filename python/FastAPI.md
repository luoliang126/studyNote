# FastAPI笔记

1. ##### fastAPI简介

   ```python
   FastAPI是一个现代的，快速（高性能）python web框架。基于标准的python类型提示，使用python3.6+构建API的Web框架。
   
   1、安装fastapi（前提已经安装python3.6+以上的版本）
   pip install fastapi
   会自动安装fastapi pydantic starlette依赖包
   pydantic:
       ujson - 更快的JSON
   	email_validator - 电子邮件的验证
   starlette:
       requests - 如果你想要使用TestClient, 需要导入requests.
       aiofiles - 如果你想使用FileResponse or StaticFiles, 需要导入aiofiles.
       jinja2 - 如果你想使用默认的模板配置，需要导入jinjia2.
       python-multipart -如果要使用request.form（）支持表单“解析”，则为必需。
       itsdangerous -“SessionMiddleware”支持需要。
       pyyaml - 如果需要 SchemaGenerator 支持, 则为必要.
       graphene -如果需要 GraphQLApp 支持, 则为必要.
       ujson - 如果你想使用 UJSONResponse, 则为必要.
   
   2、安装uvicorn（生产环境需要）
   Uvicorn 是一个闪电般快速的ASGI服务器，基于uvloop和httptools构建。
   ```

2. ##### 搭建项目

   ```python
   1、创建main.py文件
   from fastapi import FastAPI
   app = FastAPI()
   @app.get("/")
   def read_root():
       return {"Hello": "World"}
   
   @app.get("/items/{item_id}")
   def read_item(item_id: int, q: str = None):
       return {"item_id": item_id, "q": q}
   
   2、运行项目
   uvicorn main:app --reload
   访问路径：
   http://127.0.0.1:8000    //输出 { "Hello": "World" }
   http://127.0.0.1:8000/items/11123 //输出 {"item_id":11123,"q":null}
   ```

3. 虚位以待！

