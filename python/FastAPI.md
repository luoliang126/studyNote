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
   
   // 同步方式async await
   @app.get("/items/{item_id}")
   async def read_item(item_id):
       return {"item_id": item_id}
   
   2、运行项目
   uvicorn main:app --reload
   访问路径：
   http://127.0.0.1:8000    //输出 { "Hello": "World" }
   http://127.0.0.1:8000/items/11123 //输出 {"item_id":11123,"q":null}
   修改访问端口：
   uvicorn main:app --host '0.0.0.0'--port 8080 --reload
       
   3、执行命令后会自动生成api文档（太爽了）
   访问路径： http://127.0.0.1:8000/docs
   也可以： http://127.0.0.1:8000/redoc
   ```

3. ##### 基本使用

   ```python
   1、传参
   #路径传参：
   @app.get("/items/{item_id}")
   async def read_item(item_id):
       return {"item_id": item_id}
   // 指定参数数据类型
   @app.get("/items/{item_id}")
   async def read_item(item_id: int, q: str = None): // q类型是str,默认值是None，证明这个参数可选
       return {"item_id": item_id}
   http://127.0.0.1:8000/items/foo // 抛错，并提示错误原因，类型错误
   注意：如果item_id指定了类型int，会验证传入参数类型，类型不同会抛错
   http://127.0.0.1:8000/items/123 // 正确方式
   
   #拼接地址栏传参
   from fastapi import FastAPI
   app = FastAPI()
   fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]
   @app.get("/items/")
   async def read_item(skip: int = 0, limit: int = 10):
       return fake_items_db[skip : skip + limit] // 返回fake_items_db[0:10]数组从第1到第10个
   访问路径：
   http://127.0.0.1:8000/items?skip=0&limit=10
           
   #请求体body传参
   from fastapi import FastAPI
   from pydantic import BaseModel // 引入BaseModel
   class Item(BaseModel):		   // 定义数据模型，如果没有定义默认值str=None,那么该参数必传，否则抛错！
       name: str
       description: str = None
       price: float
       tax: float = None
   @app.post("/items/")
   async def create_item(item: Item):
       return item;
   注意：body传参一个对象很好实现，但是如果有嵌套怎么办？
   item={
   	name: 'luoliang'
       description: '描述内容'
       price: 10
       tax: 0.2
   }
   {
       detail:{
           name: 'luoliang'
           description: '描述内容'
           price: 10
           tax: 0.2
       }
   }
   解决办法：使用Body
   
   
   #地址栏+请求体混合参数模式
   from fastapi import FastAPI
   from pydantic import BaseModel
   class Item(BaseModel):
       name: str
       description: str = None
       price: float
       tax: float = None
   @app.put("/items/{item_id}")
   async def create_item(item_id: int, item: Item):
       return {"item_id": item_id, **item.dict()} // **item.dict()将item转化为字典
   注意：混合get和post的方式传参。
   
   2、参数校验
   #单个传参方式校验
   from fastapi import FastAPI,Path,Query  // 引入Path,Query
   @app.get("/items/")
   async def read_items(q: str = Query(None, max_length=50)):
       results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
       if q:
           results.update({"q": q})
       return results
   注意：q: str = Query(None,min_length=10,max_length=50) // 查询参数q默认值为None,最大长度10,最大长度50,选填
       q: str = Query(...,min_length=10,max_length=50) // ...... q没有默认值，必填
   
   #多个传参方式校验
   @app.get("/items/{item_id}")
   async def read_items(
       item_id:int = Path(...,title="标题描述",ge=50,le=100), // 大于等于50，小于等于100
       q: str = Query(None, alias="item-query"), // alias="item-query" ,原本参数q，现在改为别名item-query
       size:float = Query(1,gt=0,lt=10.5) // 大于0，小于10.5
   ):
       results = { "item_id":item_id }
       if q:
           results.update({"q": q})
       return results
   访问：http://localhost:8000/items/50?item-query=qwe&size=10 //50是地址栏，item-query、size是地址栏参数
   注意：item_id:int = Path(...,title="标题描述",ge=50,le=100) 这里使用的是Path，因为item_id是地址栏传参
       q: str = Query(None, alias="item-query"),
   
   #body请求提的参数校验
   
   
   ```

4. 虚位以待！
