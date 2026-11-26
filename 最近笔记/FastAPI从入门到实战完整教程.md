**FastAPI从入门到实战完整教程**

---

## 目录

1. [导学课程](#1-fastapi从入门到实战导学课程)
2. [框架简介](#2-fastapi基础入门-fastapi框架简介)
3. [第一个FastAPI程序](#3-fastapi基础入门-第一个fastapi程序)
4. [路由](#4-fastapi基础入门-路由)
5. [参数简介和路径参数](#5-fastapi基础入门-参数简介和路径参数)
6. [路径参数_Path类型注解](#6-fastapi基础入门-路径参数_path类型注解)
7. [查询参数和Query类型注解](#7-fastapi基础入门-查询参数和query类型注解)
8. [请求体参数](#8-fastapi基础入门-请求体参数)
9. [请求体参数_Field类型注解](#9-fastapi基础入门-请求体参数_field类型注解)
10. [响应类型-JSON格式](#10-fastapi基础入门-响应类型-json格式)
11. [响应类型-HTML格式](#11-fastapi基础入门-响应类型-html格式)
12. [响应类型-文件格式](#12-fastapi基础入门-响应类型-文件格式)
13. [自定义响应数据格式](#13-fastapi基础入门-自定义响应数据格式)
14. [异常响应处理](#14-fastapi基础入门-异常响应处理)
15. [中间件](#15-fastapi进阶-中间件)
16. [依赖注入](#16-fastapi进阶-依赖注入)
17. [ORM简介及安装](#17-fastapi进阶-orm简介及安装)
18. [ORM-建表](#18-fastapi进阶-orm-建表)
19. [ORM-在路由中使用ORM](#19-fastapi进阶-orm-在路由中使用orm)
20. [ORM操作数据-查询数据](#20-fastapi进阶-orm操作数据-查询数据)
21. [ORM操作数据-条件查询-比较判断](#21-fastapi进阶-orm操作数据-条件查询-比较判断)
22. [ORM操作数据-条件查询-模糊&与非&包含](#22-fastapi进阶-orm操作数据-条件查询-模糊与非包含)
23. [ORM操作数据-聚合查询](#23-fastapi进阶-orm操作数据-聚合查询)
24. [ORM操作数据-分页查询](#24-fastapi进阶-orm操作数据-分页查询)
25. [ORM操作数据-查询总结](#25-fastapi进阶-orm操作数据-查询总结)
26. [ORM操作数据-新增数据](#26-fastapi进阶-orm操作数据-新增数据)
27. [ORM操作数据-更新数据](#27-fastapi进阶-orm操作数据-更新数据)
28. [ORM操作数据-删除数据](#28-fastapi进阶-orm操作数据-删除数据)
29. [ORM总结](#29-fastapi进阶-orm总结)

---

## 1. FastAPI从入门到实战导学课程

### 课程概述

本课程将带领您从零基础开始学习FastAPI框架，逐步深入到高级特性和实战应用。

#### 课程目标

- 掌握FastAPI框架的核心概念和基本用法
- 能够独立开发RESTful API服务
- 学会使用SQLAlchemy进行数据库操作
- 理解中间件和依赖注入的原理与应用

#### 学习路径

```
基础入门 → 参数处理 → 响应类型 → 进阶特性 → ORM实战
```

#### 课程结构

| 阶段 | 章节范围 | 主要内容 |
|------|----------|----------|
| 基础入门 | 02-08 | 框架简介、路由、参数处理 |
| 响应处理 | 09-13 | 响应类型、异常处理 |
| 进阶特性 | 14-15 | 中间件、依赖注入 |
| ORM实战 | 16-28 | SQLAlchemy、数据操作 |

#### 环境准备

在开始学习前，请确保您的系统已安装：

1. **Python 3.12+**（2026年最主流版本，支持最新特性）
2. **pip** 包管理工具
3. **Anaconda/Miniconda**（推荐用于环境管理）

#### 使用Conda创建FastAPI环境

##### 1. 创建环境

```bash
# 创建名为fastapi_env的环境，指定Python 3.12版本
conda create -n fastapi_env python=3.12 -y
```

##### 2. 激活环境

```bash
# Windows
conda activate fastapi_env

# macOS/Linux
source activate fastapi_env
```

##### 3. 安装FastAPI和Uvicorn

```bash
pip install "fastapi>=0.110.0" "uvicorn>=0.30.0"
```

##### 4. 安装ORM相关依赖（可选，用于后续章节）

```bash
pip install "sqlalchemy>=2.0.0" "aiosqlite>=0.20.0"
```

##### 5. 退出环境（可选）

```bash
conda deactivate
```

#### 使用venv创建环境（备选方案）

如果您没有安装Conda，可以使用Python内置的venv：

```bash
# 创建环境目录
python -m venv fastapi_env

# 激活环境
# Windows
fastapi_env\Scripts\activate

# macOS/Linux
source fastapi_env/bin/activate

# 安装依赖
pip install "fastapi>=0.110.0" "uvicorn>=0.30.0"
```

#### 课程特色

- **实战导向**：每个知识点都配有完整案例
- **代码驱动**：通过代码理解概念
- **循序渐进**：从基础到高级，层层递进

让我们开始FastAPI的学习之旅！

---

## 2. FastAPI基础入门 - FastAPI框架简介

### 什么是FastAPI

FastAPI是一个现代、快速（高性能）的Web框架，用于构建API，基于Python 3.10+的类型提示（2026年推荐使用Python 3.12+）。

#### 框架特点

##### 1. 快速
- 极高的性能，与Node.js和Go相当
- 基于Starlette和Pydantic构建

##### 2. 高效编码
- 自动生成交互式API文档（Swagger UI和ReDoc）
- 强大的类型提示支持
- 减少代码重复

##### 3. 健壮性
- 自动请求验证
- 自动响应序列化
- 内置依赖注入系统

##### 4. 现代化
- 完全支持Python 3.10+类型提示（推荐Python 3.12+）
- 异步支持
- 符合OpenAPI标准

#### FastAPI vs 其他框架

| 框架 | 性能 | 类型提示 | 自动文档 | 异步支持 |
|------|------|----------|----------|----------|
| FastAPI | 极高 | 原生支持 | 自动生成 | 原生支持 |
| Flask | 中等 | 插件支持 | 手动配置 | 通过协程 |
| Django | 中等 | 插件支持 | 手动配置 | 有限支持 |

#### 核心组件

1. **Starlette**：负责HTTP请求处理
2. **Pydantic**：负责数据验证和序列化
3. **OpenAPI**：API规范标准

#### 适用场景

- 构建RESTful API服务
- 微服务架构
- 实时数据处理
- 机器学习模型部署

#### 本章小结

FastAPI凭借其高性能、易用性和现代化特性，已成为Python Web开发的热门选择。下一章我们将编写第一个FastAPI程序。

---

## 3. FastAPI基础入门 - 第一个FastAPI程序

### 安装依赖

首先需要安装FastAPI和ASGI服务器：

```bash
pip install "fastapi>=0.110.0" "uvicorn>=0.30.0"
```

### 创建第一个程序

#### 代码示例

```python
from fastapi import FastAPI

# 创建FastAPI实例
app = FastAPI()

#这是上面的简单点的接口
# 定义路由
@app.get("/")
# 异步函数
async def read_root():
    return {"message": "Hello, FastAPI!"}
```

#### 代码解析

1. **导入FastAPI**：从`fastapi`模块导入`FastAPI`类
2. **创建实例**：`app = FastAPI()` 创建应用实例
3. **定义路由**：使用装饰器`@app.get("/")`定义GET请求路由
4. **响应内容**：返回字典，FastAPI会自动转换为JSON

### 运行服务

```bash
uvicorn main:app --reload
```

参数说明：
- `main`：Python文件名（不含`.py`）
- `app`：FastAPI实例名
- `--reload`：开发模式，文件修改后自动重启,最好加上

### 访问服务

服务启动后，默认运行在 `http://127.0.0.1:8000`

#### 访问API端点

```bash
curl http://127.0.0.1:8000/
```

响应结果：
```json
{"message": "Hello, FastAPI!"}
```

#### 浏览器访问方式

**在浏览器地址栏输入：**
```
http://127.0.0.1:8000/
```

**预期返回：**
```json
{"message": "Hello, FastAPI!"}
```

### 交互式文档

FastAPI自动生成交互式API文档：

#### Swagger UI
访问地址：`http://127.0.0.1:8000/docs`

#### ReDoc
访问地址：`http://127.0.0.1:8000/redoc`

### 本章小结

通过简单的几行代码，我们创建了一个完整的FastAPI应用。关键知识点：

1. 创建`FastAPI`实例
2. 使用装饰器定义路由
3. 运行服务使用`uvicorn`
4. 自动生成API文档

下一章将深入学习路由的概念和用法。

---

## 4. FastAPI基础入门 - 路由

### 路由概述

路由就是URL地址和处理函数之间的映射关系，它决定了当用户访问某个特定网址时，服务器应该执行哪段代码(函数)来返回结果。
那网址和函数怎么关联起来呢:用装饰器
在FastAPI中，路由通过装饰器定义:@app.get("/")。

![[FastAPI-01.png]]


同一张图,测试一下图片路径对上传的影响
![FastAPI-01](assets/FastAPI-01.png)

### HTTP请求方法

FastAPI支持所有常用的HTTP方法：

| 方法     | 用途   | 装饰器             |
| ------ | ---- | --------------- |
| GET    | 获取资源 | `@app.get()`    |
| POST   | 创建资源 | `@app.post()`   |
| PUT    | 更新资源 | `@app.put()`    |
| DELETE | 删除资源 | `@app.delete()` |
| PATCH  | 部分更新 | `@app.patch()`  |

### 路由定义示例

#### 基础路由

```python
from fastapi import FastAPI

app = FastAPI()

#原始
@app.get("/")  #这里/表示根路径
def root():
    return {"message": "Hello, FastAPI!"}

#访问 /hello 响应结果  msg: 你好 FastAPI
@app.get("/hello")  
async def say_hello():  
    return {"msg": "你好  FastAPI"}
    
@app.post("/items/")
def create_item():
    return {"message": "Item created"}

@app.put("/items/{item_id}")
def update_item(item_id: int):
    return {"item_id": item_id, "message": "Item updated"}


@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    return {"item_id": item_id, "message": "Item deleted"}
```

#### 浏览器访问方式

| 路由 | 浏览器地址 | 预期返回 |
|------|-----------|---------|
| `GET /` | `http://127.0.0.1:8000/` | `{"message": "Hello, FastAPI!"}` |
| `GET /items/1` | `http://127.0.0.1:8000/items/1` | `{"item_id": 1, "message": "Item updated"}` |
| `DELETE /items/5` | 使用Swagger文档或工具发送DELETE请求 | `{"item_id": 5, "message": "Item deleted"}` |

> **注意**：POST、PUT、DELETE等请求无法直接在浏览器地址栏输入，需要使用Swagger文档（`http://127.0.0.1:8000/docs`）或API测试工具（如Postman、curl）发送。

#### 路径参数

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/users/123` → `{"user_id": 123}`

#### 路由顺序

FastAPI按照路由定义的顺序进行匹配，因此需要将更具体的路由放在前面：

```python
# 正确顺序
@app.get("/items/me")
def get_current_item():
    return {"item_id": "current"}

@app.get("/items/{item_id}")
def get_item(item_id: str):
    return {"item_id": item_id}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/me` → `{"item_id": "current"}`
- `http://127.0.0.1:8000/items/abc123` → `{"item_id": "abc123"}`

### 路径操作函数

#### 函数命名建议

- 使用动词开头：`get_item`, `create_user`
- 使用清晰的命名：`get_current_user` 而非 `get_user_1`

#### 返回类型

路径操作函数可以返回：
- 字典/列表
- Pydantic模型
- 响应对象
- 字符串

### 本章小结

路由是FastAPI的核心概念，通过装饰器定义。关键点：

1. 支持多种HTTP方法
2. 路径参数通过`{param}`定义
3. 路由顺序影响匹配结果
4. 返回值自动序列化为JSON

下一章将学习参数的处理方式。

---

## 5. FastAPI基础入门 - 参数简介和路径参数

### 参数类型简介

FastAPI支持多种类型的参数：

| 参数类型 | 来源 | 示例 |
|----------|------|------|
| 路径参数 | URL路径 | `/items/{item_id}` |
| 查询参数 | URL查询 | `/items/?skip=0&limit=10` |
| 请求体参数 | 请求正文 | JSON数据 |
| 表单参数 | Form表单 | 表单提交 |
| 文件参数 | 文件上传 | 上传文件 |

### 路径参数

路径参数是URL路径的一部分，用于标识特定资源。

#### 基本用法

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}
```

**浏览器访问方式：**

- `http://127.0.0.1:8000/items/1` → `{"item_id": 1}`
- `http://127.0.0.1:8000/items/100` → `{"item_id": 100}`

#### 类型转换

FastAPI会自动进行类型转换和验证：

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    # 如果传入非数字，会自动返回422错误
    return {"user_id": user_id, "type": type(user_id).__name__}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/users/456` → `{"user_id": 456, "type": "int"}`
- `http://127.0.0.1:8000/users/abc` → 返回422验证错误

#### 多个路径参数

```python
@app.get("/users/{user_id}/items/{item_id}")
def read_user_item(user_id: int, item_id: str):
    return {"user_id": user_id, "item_id": item_id}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/users/10/items/laptop` → `{"user_id": 10, "item_id": "laptop"}`

#### 预定义路径参数

使用枚举限制可选值：

```python
from enum import Enum

class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"

@app.get("/models/{model_name}")
def get_model(model_name: ModelName):
    if model_name == ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}
    
    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}
    
    return {"model_name": model_name, "message": "Have some residuals"}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/models/alexnet` → `{"model_name": "alexnet", "message": "Deep Learning FTW!"}`
- `http://127.0.0.1:8000/models/resnet` → `{"model_name": "resnet", "message": "Have some residuals"}`
- `http://127.0.0.1:8000/models/lenet` → `{"model_name": "lenet", "message": "LeCNN all the images"}`
- `http://127.0.0.1:8000/models/unknown` → 返回422验证错误



### 本章小结

路径参数是构建RESTful API的基础：

1. 通过`{param}`定义路径参数
2. FastAPI自动进行类型转换和验证
3. 支持枚举类型限制可选值
4. 使用`path`转换器处理包含路径的参数

下一章将学习Path类型注解的高级用法。

---

## 6. FastAPI基础入门 - 路径参数_Path类型注解

### Path类型注解概述

`Path`类用于对路径参数进行更详细的验证和元数据配置。

### 导入Path

```python
from fastapi import FastAPI, Path
```

### 基本用法

#### 添加验证约束

```python
from fastapi import FastAPI, Path

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(
    item_id: int = Path(..., ge=1, le=1000),
    q: str = None
):
    return {"item_id": item_id, "q": q}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/42` → `{"item_id": 42, "q": null}`
- `http://127.0.0.1:8000/items/42?q=hello` → `{"item_id": 42, "q": "hello"}`
- `http://127.0.0.1:8000/items/0` → 返回422验证错误（item_id必须≥1）
- `http://127.0.0.1:8000/items/2000` → 返回422验证错误（item_id必须≤1000）

#### 参数说明

- `...`：表示参数是必需的（必需路径参数）
- `ge=1`：大于等于1
- `le=1000`：小于等于1000

### Path参数完整分类

Path参数支持以下四类配置：

#### 一、第一位置参数：默认值/必填标记

```python
item_id: int = Path(...)          # 必填参数
item_id: int = Path(None)         # 可选参数，默认值为None
item_id: int = Path(10)           # 可选参数，默认值为10
```

#### 二、校验属性（验证规则）—— 触发参数校验，非法值返回422

**数字类型适用（int/float）**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `gt` | greater than，大于 | `gt=0` |
| `ge` | greater equal，大于等于 | `ge=1` |
| `lt` | less than，小于 | `lt=100` |
| `le` | less equal，小于等于 | `le=1000` |

**字符串适用**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `min_length` | 最小长度 | `min_length=1` |
| max_length` | 最大长度 | `max_length=50` |
| `regex` | 正则表达式匹配（旧版本） | `regex="^[a-z]+$"` |
| `pattern` | 正则表达式（新版Pydantic标准） | `pattern="^[a-z]+$"` |

**数组/列表类型**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `min_items` | 列表最少元素数 | `min_items=1` |
| `max_items` | 列表最多元素数 | `max_items=10` |
| `unique_items` | 元素是否唯一 | `unique_items=True` |

#### 三、元信息属性 —— 只用于生成接口文档，不参与任何校验

| 参数 | 说明 | 示例 |
|------|------|------|
| `title` | 参数标题 | `title="Item ID"` |
| `description` | 参数详细描述 | `description="Must be between 1 and 1000"` |
| `example` | 单个示例值 | `example=42` |
| `examples` | 多个示例（字典格式） | `examples={"small": {"value": 1}, "large": {"value": 1000}}` |
| `deprecated` | bool，标记参数已废弃 | `deprecated=True` |

#### 四、其他通用配置（进阶）

| 参数 | 说明 | 示例 |
|------|------|------|
| `alias` | 参数别名 | `alias="item-id"` |
| `include_in_schema` | 是否在文档中显示该参数 | `include_in_schema=False` |

### 示例：综合使用

```python
@app.get("/items/{item_id}")
def read_item(
    item_id: int = Path(
        ...,
        title="The ID of the item",
        description="Must be between 1 and 1000",
        ge=1,
        le=1000
    ),
    q: str = None
):
    return {"item_id": item_id, "q": q}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/500?q=test` → `{"item_id": 500, "q": "test"}`

### 多个路径参数

```python
@app.get("/users/{user_id}/items/{item_id}")
def read_user_item(
    user_id: int = Path(..., ge=1, title="User ID"),
    item_id: int = Path(..., ge=1, title="Item ID")
):
    return {"user_id": user_id, "item_id": item_id}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/users/100/items/200` → `{"user_id": 100, "item_id": 200}`

### 与查询参数混合

```python
@app.get("/items/{item_id}")
def read_item(
    *,  # 强制使用关键字参数
    item_id: int = Path(..., ge=1),
    q: str = None,
    size: float = Path(..., gt=0, lt=10.5)
):
    return {"item_id": item_id, "q": q, "size": size}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/100/5.5` → `{"item_id": 100, "q": null, "size": 5.5}`
- `http://127.0.0.1:8000/items/100/5.5?q=hello` → `{"item_id": 100, "q": "hello", "size": 5.5}`

### 本章小结

Path类型注解提供了强大的参数验证能力：

1. 使用`Path`类添加验证约束
2. 支持数值范围验证（ge, gt, le, lt）
3. 可以添加元数据（title, description）
4. 使用`*`强制关键字参数顺序

下一章将学习查询参数和Query类型注解。

---

## 7. FastAPI基础入门 - 查询参数和Query类型注解

### 查询参数概述

查询参数是URL中`?`后面的键值对，用于过滤、分页等操作。

### 基本用法

```python
from fastapi import FastAPI

app = FastAPI()

fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]

@app.get("/items/")
def read_items(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/` → 返回所有3个item
- `http://127.0.0.1:8000/items/?skip=0&limit=2` → `[{"item_name": "Foo"}, {"item_name": "Bar"}]`
- `http://127.0.0.1:8000/items/?skip=1&limit=1` → `[{"item_name": "Bar"}]`

### Query类型注解

`Query`类用于对查询参数进行更详细的验证和配置。

#### 导入Query

```python
from fastapi import FastAPI, Query
```

#### 基本用法

```python
@app.get("/items/")
def read_items(
    q: str = Query(None, min_length=3, max_length=50),
    skip: int = Query(0, ge=0),
    limit: int = Query(10, ge=0, le=100)
):
    results = {"items": fake_items_db[skip : skip + limit]}
    if q:
        results.update({"q": q})
    return results
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/?q=hello&skip=0&limit=2` → `{"items": [{"item_name": "Foo"}, {"item_name": "Bar"}], "q": "hello"}`
- `http://127.0.0.1:8000/items/?q=ab` → 返回422验证错误（q长度不足3）

### Query参数完整分类

Query参数支持以下四类配置：

#### 一、第一位置参数：默认值/必填标记

```python
q: str = Query(...)              # 必填参数
q: str = Query(None)             # 可选参数，默认值为None
q: str = Query("default")        # 可选参数，默认值为"default"
q: list = Query([])              # 可选列表参数，默认空列表
q: list = Query(["foo", "bar"])  # 可选列表参数，有默认值
```

#### 二、校验属性（验证规则）—— 触发参数校验，非法值返回422

**数字类型适用（int/float）**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `gt` | greater than，大于 | `gt=0` |
| `ge` | greater equal，大于等于 | `ge=1` |
| `lt` | less than，小于 | `lt=100` |
| `le` | less equal，小于等于 | `le=1000` |

**字符串适用**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `min_length` | 最小长度 | `min_length=3` |
| `max_length` | 最大长度 | `max_length=50` |
| `regex` | 正则表达式匹配（旧版本） | `regex="^fixedquery$"` |
| `pattern` | 正则表达式（新版Pydantic标准） | `pattern="^[a-z]+$"` |

**数组/列表类型**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `min_items` | 列表最少元素数 | `min_items=1` |
| `max_items` | 列表最多元素数 | `max_items=10` |
| `unique_items` | 元素是否唯一 | `unique_items=True` |

#### 三、元信息属性 —— 只用于生成接口文档，不参与任何校验

| 参数 | 说明 | 示例 |
|------|------|------|
| `title` | 参数标题 | `title="Search query"` |
| `description` | 参数详细描述 | `description="Search string to filter items"` |
| `example` | 单个示例值 | `example="search term"` |
| `examples` | 多个示例（字典格式） | `examples={"simple": {"value": "hello"}, "complex": {"value": "advanced search"}}` |
| `deprecated` | bool，标记参数已废弃 | `deprecated=True` |

#### 四、其他通用配置（进阶）

| 参数 | 说明 | 示例 |
|------|------|------|
| `alias` | 参数别名 | `alias="search-query"` |
| `include_in_schema` | 是否在文档中显示该参数 | `include_in_schema=False` |

### 必需查询参数

```python
@app.get("/items/")
def read_items(q: str = Query(..., min_length=3)):
    # ...表示必需参数
    return {"q": q}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/?q=search` → `{"q": "search"}`
- `http://127.0.0.1:8000/items/` → 返回422验证错误（q是必需参数）

### 列表查询参数

```python
@app.get("/items/")
def read_items(q: list = Query([])):
    # 支持多个相同参数：?q=foo&q=bar
    return {"q": q}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/?q=foo&q=bar` → `{"q": ["foo", "bar"]}`
- `http://127.0.0.1:8000/items/` → `{"q": []}`

### 带默认值的列表参数

```python
@app.get("/items/")
def read_items(q: list = Query(["foo", "bar"])):
    return {"q": q}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/` → `{"q": ["foo", "bar"]}`（使用默认值）
- `http://127.0.0.1:8000/items/?q=apple&q=orange` → `{"q": ["apple", "orange"]}`

### 本章小结

查询参数是API中常用的参数类型：

1. 查询参数位于URL的`?`之后
2. 使用`Query`类进行高级验证
3. 支持字符串长度、数值范围等验证
4. 支持列表类型参数

下一章将学习Header和Cookie参数。

---

## 7.1 FastAPI基础入门 - Header和Cookie参数完整分类

### Header参数

`Header`类用于获取和验证HTTP请求头中的参数。

#### 导入Header

```python
from fastapi import FastAPI, Header
```

#### Header参数完整分类

**一、第一位置参数：默认值/必填标记**

```python
x_token: str = Header(...)          # 必填Header
x_token: str = Header(None)         # 可选Header，默认值为None
x_token: str = Header("default")    # 可选Header，默认值为"default"
```

**二、校验属性（验证规则）**

| 参数 | 说明 | 示例 |
|------|------|------|
| `gt` | 大于 | `gt=0` |
| `ge` | 大于等于 | `ge=1` |
| `lt` | 小于 | `lt=100` |
| `le` | 小于等于 | `le=1000` |
| `min_length` | 最小长度 | `min_length=1` |
| `max_length` | 最大长度 | `max_length=100` |
| `regex` | 正则表达式（旧版本） | `regex="^Bearer .*"` |
| `pattern` | 正则表达式（新版） | `pattern="^Bearer .*"` |

**三、元信息属性**

| 参数 | 说明 | 示例 |
|------|------|------|
| `title` | 参数标题 | `title="Authorization Token"` |
| `description` | 参数描述 | `description="API authorization token"` |
| `example` | 示例值 | `example="Bearer abc123"` |
| `deprecated` | 是否废弃 | `deprecated=True` |

**四、其他通用配置**

| 参数 | 说明 | 示例 |
|------|------|------|
| `alias` | 参数别名 | `alias="X-Token"` |
| `include_in_schema` | 是否在文档中显示 | `include_in_schema=False` |
| `convert_underscores` | 是否自动将下划线转换为连字符 | `convert_underscores=True` |

#### 示例：使用Header参数

```python
@app.get("/items/")
async def read_items(
    x_token: str = Header(..., description="Authorization token"),
    x_key: str = Header(None)
):
    return {"X-Token": x_token, "X-Key": x_key}
```

**浏览器访问方式**（使用curl携带Header）：
- `curl -H "X-Token: my-secret-token" http://127.0.0.1:8000/items/` → `{"X-Token": "my-secret-token", "X-Key": null}`

### Cookie参数

`Cookie`类用于获取和验证HTTP请求中的Cookie参数。

#### 导入Cookie

```python
from fastapi import FastAPI, Cookie
```

#### Cookie参数完整分类

**一、第一位置参数：默认值/必填标记**

```python
session_id: str = Cookie(...)          # 必填Cookie
session_id: str = Cookie(None)         # 可选Cookie，默认值为None
session_id: str = Cookie("default")    # 可选Cookie，默认值为"default"
```

**二、校验属性（验证规则）**

| 参数 | 说明 | 示例 |
|------|------|------|
| `gt` | 大于 | `gt=0` |
| `ge` | 大于等于 | `ge=1` |
| `lt` | 小于 | `lt=100` |
| `le` | 小于等于 | `le=1000` |
| `min_length` | 最小长度 | `min_length=1` |
| `max_length` | 最大长度 | `max_length=100` |
| `regex` | 正则表达式（旧版本） | `regex="^[a-z0-9]+$"` |
| `pattern` | 正则表达式（新版） | `pattern="^[a-z0-9]+$"` |

**三、元信息属性**

| 参数 | 说明 | 示例 |
|------|------|------|
| `title` | 参数标题 | `title="Session ID"` |
| `description` | 参数描述 | `description="User session identifier"` |
| `example` | 示例值 | `example="abc123def456"` |
| `deprecated` | 是否废弃 | `deprecated=True` |

**四、其他通用配置**

| 参数 | 说明 | 示例 |
|------|------|------|
| `alias` | 参数别名 | `alias="session-id"` |
| `include_in_schema` | 是否在文档中显示 | `include_in_schema=False` |

#### 示例：使用Cookie参数

```python
@app.get("/users/me")
async def read_user_me(session_id: str = Cookie(None)):
    return {"session_id": session_id}
```

**浏览器访问方式**（使用curl携带Cookie）：
- `curl -b "session_id=abc123" http://127.0.0.1:8000/users/me` → `{"session_id": "abc123"}`

---

## 8. FastAPI基础入门 - 请求体参数

### 请求体概述

请求体是客户端发送给API的数据，通常用于POST、PUT、PATCH等请求。

### Body参数完整分类

`Body`类用于直接从请求体中获取单个字段，而不是使用Pydantic模型。

#### 导入Body

```python
from fastapi import FastAPI, Body
```

#### Body参数完整分类

**一、第一位置参数：默认值/必填标记**

```python
name: str = Body(...)          # 必填字段
name: str = Body(None)         # 可选字段，默认值为None
name: str = Body("default")    # 可选字段，默认值为"default"
```

**二、校验属性（验证规则）**

| 参数 | 说明 | 示例 |
|------|------|------|
| `gt` | 大于 | `gt=0` |
| `ge` | 大于等于 | `ge=1` |
| `lt` | 小于 | `lt=100` |
| `le` | 小于等于 | `le=1000` |
| `min_length` | 最小长度 | `min_length=1` |
| `max_length` | 最大长度 | `max_length=100` |
| `regex` | 正则表达式（旧版本） | `regex="^[a-z]+$"` |
| `pattern` | 正则表达式（新版） | `pattern="^[a-z]+$"` |
| `min_items` | 列表最少元素数 | `min_items=1` |
| `max_items` | 列表最多元素数 | `max_items=10` |
| `unique_items` | 元素是否唯一 | `unique_items=True` |

**三、元信息属性**

| 参数 | 说明 | 示例 |
|------|------|------|
| `title` | 参数标题 | `title="Item name"` |
| `description` | 参数描述 | `description="Name of the item"` |
| `example` | 示例值 | `example="Foo"` |
| `examples` | 多个示例 | `examples={"simple": {"value": "Hello"}}` |
| `deprecated` | 是否废弃 | `deprecated=True` |

**四、其他通用配置**

| 参数 | 说明 | 示例 |
|------|------|------|
| `alias` | 参数别名 | `alias="item-name"` |
| `include_in_schema` | 是否在文档中显示 | `include_in_schema=False` |
| `embed` | 是否嵌入到JSON对象中 | `embed=True` |

#### 示例：使用Body参数

```python
@app.put("/items/{item_id}")
async def update_item(
    item_id: int,
    name: str = Body(..., description="Item name"),
    price: float = Body(None, ge=0)
):
    return {"item_id": item_id, "name": name, "price": price}
```

**浏览器访问方式**（使用curl）：
- `curl -X PUT "http://127.0.0.1:8000/items/42" -H "Content-Type: application/json" -d '{"name": "Updated Item", "price": 19.99}'` → `{"item_id": 42, "name": "Updated Item", "price": 19.99}`

### 使用Pydantic模型

FastAPI使用Pydantic模型来定义请求体结构。

#### 基本用法

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None

@app.post("/items/")
def create_item(item: Item):
    return item
```

#### 请求示例

```bash
curl -X POST "http://127.0.0.1:8000/items/" -H "Content-Type: application/json" -d '{
    "name": "Foo",
    "description": "A very nice Item",
    "price": 35.4,
    "tax": 3.2
}'
```

#### 响应示例

```json
{
    "name": "Foo",
    "description": "A very nice Item",
    "price": 35.4,
    "tax": 3.2
}
```

#### 浏览器访问方式

> **注意**：POST请求无法直接在浏览器地址栏输入，需要使用以下方式测试：

**方式一：使用Swagger文档**
1. 打开 `http://127.0.0.1:8000/docs`
2. 找到 `/items/` POST接口
3. 点击"Try it out"按钮
4. 在请求体中输入JSON数据
5. 点击"Execute"发送请求

**方式二：使用curl命令**
```bash
curl -X POST "http://127.0.0.1:8000/items/" -H "Content-Type: application/json" -d '{
    "name": "Foo",
    "description": "A very nice Item",
    "price": 35.4,
    "tax": 3.2
}'
```

**方式三：使用API测试工具**（如Postman、Insomnia）

### Pydantic模型特性

#### 类型验证

```python
class User(BaseModel):
    username: str
    email: str
    full_name: str = None
    disabled: bool = None

@app.post("/users/")
def create_user(user: User):
    # FastAPI自动验证请求体数据类型
    return {"username": user.username, "email": user.email}
```

**浏览器访问方式**（使用Swagger文档或curl）：
```bash
curl -X POST "http://127.0.0.1:8000/users/" -H "Content-Type: application/json" -d '{
    "username": "john_doe",
    "email": "john@example.com"
}'
```

#### 嵌套模型

```python
class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None

class Order(BaseModel):
    items: list[Item]
    total: float

@app.post("/orders/")
def create_order(order: Order):
    return order
```

**浏览器访问方式**（使用Swagger文档或curl）：
```bash
curl -X POST "http://127.0.0.1:8000/orders/" -H "Content-Type: application/json" -d '{
    "items": [
        {"name": "Foo", "price": 35.4},
        {"name": "Bar", "price": 20.0}
    ],
    "total": 55.4
}'
```

#### 多个请求体参数

```python
@app.post("/items/{item_id}")
def update_item(item_id: int, item: Item, user: User):
    return {"item_id": item_id, "item": item, "user": user}
```

**浏览器访问方式**（使用Swagger文档或curl）：
```bash
curl -X POST "http://127.0.0.1:8000/items/1" -H "Content-Type: application/json" -d '{
    "item": {"name": "Foo", "price": 35.4},
    "user": {"username": "john", "email": "john@example.com"}
}'
```

### 与路径参数、查询参数混合

```python
@app.post("/items/{item_id}")
def update_item(
    item_id: int,
    item: Item,
    q: str = None
):
    result = {"item_id": item_id, **item.dict()}
    if q:
        result.update({"q": q})
    return result
```

**浏览器访问方式**（使用Swagger文档或curl）：
```bash
curl -X POST "http://127.0.0.1:8000/items/1?q=special" -H "Content-Type: application/json" -d '{
    "name": "Foo",
    "price": 35.4
}'
```

### 本章小结

请求体参数是创建和更新资源的关键：

1. 使用Pydantic的`BaseModel`定义请求体结构
2. FastAPI自动进行数据验证和类型转换
3. 支持嵌套模型和多个请求体参数
4. 可以与路径参数、查询参数混合使用

下一章将学习Field类型注解的高级用法。

---

## 9. FastAPI基础入门 - 请求体参数_Field类型注解

### Field类型注解概述

`Field`类用于在Pydantic模型中对字段进行更详细的验证和元数据配置。

### 导入Field

```python
from pydantic import BaseModel, Field
```

### 基本用法

```python
from fastapi import FastAPI
from pydantic import BaseModel, Field

app = FastAPI()

class Item(BaseModel):
    name: str = Field(..., title="Item name", min_length=1)
    description: str = Field(None, title="Item description", max_length=100)
    price: float = Field(..., gt=0, description="Price must be greater than 0")
    tax: float = Field(None, ge=0)
```

**浏览器访问方式**（使用Swagger文档或curl）：
```bash
curl -X POST "http://127.0.0.1:8000/items/" -H "Content-Type: application/json" -d '{
    "name": "Product",
    "description": "A great product",
    "price": 19.99
}'
```

### Field参数完整分类

Field参数支持以下四类配置：

#### 一、第一位置参数：默认值/必填标记

```python
name: str = Field(...)            # 必填字段
name: str = Field(None)           # 可选字段，默认值为None
name: str = Field("default")      # 可选字段，默认值为"default"
is_active: bool = Field(True)     # 可选字段，默认值为True
```

#### 二、校验属性（验证规则）—— 触发参数校验，非法值返回422

**数字类型适用（int/float）**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `gt` | greater than，大于 | `gt=0` |
| `ge` | greater equal，大于等于 | `ge=1` |
| `lt` | less than，小于 | `lt=100` |
| `le` | less equal，小于等于 | `le=1000` |
| `multiple_of` | 必须是指定数的倍数 | `multiple_of=2` |

**字符串适用**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `min_length` | 最小长度 | `min_length=1` |
| `max_length` | 最大长度 | `max_length=100` |
| `regex` | 正则表达式匹配（旧版本） | `regex="^[0-9]{6}$"` |
| `pattern` | 正则表达式（新版Pydantic标准） | `pattern="^[a-zA-Z0-9_]+$"` |

**数组/列表类型**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `min_items` | 列表最少元素数 | `min_items=1` |
| `max_items` | 列表最多元素数 | `max_items=10` |
| `unique_items` | 元素是否唯一 | `unique_items=True` |

**布尔类型**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `strict` | 是否严格类型检查 | `strict=True` |

#### 三、元信息属性 —— 只用于生成接口文档，不参与任何校验

| 参数 | 说明 | 示例 |
|------|------|------|
| `title` | 字段标题 | `title="Item name"` |
| `description` | 字段详细描述 | `description="User login name"` |
| `example` | 单个示例值 | `example="john_doe"` |
| `examples` | 多个示例（字典格式） | `examples={"valid": {"value": "admin"}, "invalid": {"value": "admin@"}}` |
| `deprecated` | bool，标记字段已废弃 | `deprecated=True` |
| `json_schema_extra` | 额外的JSON Schema字段 | `json_schema_extra={"x-custom": "value"}` |

#### 四、其他通用配置（进阶）

| 参数 | 说明 | 示例 |
|------|------|------|
| `alias` | 字段别名 | `alias="user-name"` |
| `alias_priority` | 别名优先级 | `alias_priority=1` |
| `include_in_schema` | 是否在文档中显示该字段 | `include_in_schema=False` |

### 示例：综合使用

```python
class User(BaseModel):
    username: str = Field(
        ...,
        title="Username",
        description="User login name",
        min_length=3,
        max_length=50,
        pattern="^[a-zA-Z0-9_]+$"
    )
    email: str = Field(..., title="Email", description="User email address")
    age: int = Field(None, ge=18, le=120, description="User age")
    is_active: bool = Field(True, description="User active status")

@app.post("/users/")
def create_user(user: User):
    return user
```

**浏览器访问方式**（使用Swagger文档或curl）：
```bash
curl -X POST "http://127.0.0.1:8000/users/" -H "Content-Type: application/json" -d '{
    "username": "john_doe",
    "email": "john@example.com",
    "age": 25
}'
```

### 嵌套模型中的Field

```python
class Address(BaseModel):
    street: str = Field(..., title="Street", min_length=1)
    city: str = Field(..., title="City", min_length=1)
    zip_code: str = Field(..., title="Zip code", pattern="^[0-9]{6}$")

class User(BaseModel):
    name: str = Field(..., title="Name")
    address: Address = Field(..., title="User address")

@app.post("/users/")
def create_user(user: User):
    return user
```

**浏览器访问方式**（使用Swagger文档或curl）：
```bash
curl -X POST "http://127.0.0.1:8000/users/" -H "Content-Type: application/json" -d '{
    "name": "John Doe",
    "address": {
        "street": "123 Main St",
        "city": "Beijing",
        "zip_code": "100000"
    }
}'
```

### 自定义错误消息

```python
from pydantic import Field, field_validator

class Item(BaseModel):
    name: str = Field(..., min_length=3)
    
    @field_validator('name')
    def name_must_not_be_empty(cls, v):
        if not v.strip():
            raise ValueError('Name must not be empty')
        return v
```

### 本章小结

Field类型注解增强了Pydantic模型的验证能力：

1. 使用`Field`类对模型字段进行详细配置
2. 支持多种验证约束和元数据
3. 可用于嵌套模型
4. 支持自定义验证器

下一章将学习响应类型的处理。

---

## 10. FastAPI基础入门 - 响应类型-JSON格式

### JSON响应概述

FastAPI默认将返回值序列化为JSON格式，这是RESTful API最常用的响应格式。

### 基本用法

#### 返回字典

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id, "name": "Foo", "price": 35.4}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/42` → `{"item_id": 42, "name": "Foo", "price": 35.4}`

#### 返回列表

```python
@app.get("/items/")
def read_items():
    return [
        {"item_id": 1, "name": "Foo"},
        {"item_id": 2, "name": "Bar"}
    ]
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/` → `[{"item_id": 1, "name": "Foo"}, {"item_id": 2, "name": "Bar"}]`

### 使用Pydantic模型作为响应

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    item_id: int
    name: str
    price: float
    description: str = None

@app.get("/items/{item_id}", response_model=Item)
def read_item(item_id: int):
    return {"item_id": item_id, "name": "Foo", "price": 35.4}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/42` → `{"item_id": 42, "name": "Foo", "price": 35.4, "description": null}`

### 响应模型的优势

1. **数据验证**：确保响应数据符合模型定义
2. **自动文档**：生成准确的API文档
3. **数据过滤**：自动过滤未定义的字段
4. **类型提示**：提供更好的开发体验

### 响应模型配置

#### 排除字段

```python
from pydantic import BaseModel, Field

class User(BaseModel):
    username: str
    email: str
    password: str  # 不应该返回给客户端

@app.get("/users/{user_id}", response_model=User, response_model_exclude={"password"})
def get_user(user_id: int):
    return {
        "username": "john",
        "email": "john@example.com",
        "password": "secret123"  # 会被自动排除
    }
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/users/1` → `{"username": "john", "email": "john@example.com"}`（password被排除）

#### 仅包含特定字段

```python
@app.get("/users/{user_id}", response_model=User, response_model_include={"username", "email"})
def get_user(user_id: int):
    return {
        "username": "john",
        "email": "john@example.com",
        "password": "secret123"  # 会被自动排除
    }
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/users/1` → `{"username": "john", "email": "john@example.com"}`（仅包含指定字段）

### 自定义响应状态码

```python
from fastapi import status

@app.post("/items/", status_code=status.HTTP_201_CREATED)
def create_item(item: Item):
    return {"item_id": 1, **item.dict()}
```

**浏览器访问方式**（使用Swagger文档或curl）：
```bash
curl -X POST "http://127.0.0.1:8000/items/" -H "Content-Type: application/json" -d '{
    "name": "New Item",
    "price": 19.99
}'
```
响应状态码：201 Created

### 本章小结

JSON是FastAPI最常用的响应格式：

1. 默认自动序列化返回值为JSON
2. 使用`response_model`指定响应模型
3. 支持排除或包含特定字段
4. 可以自定义响应状态码

下一章将学习HTML格式的响应。

---

## 11. FastAPI基础入门 - 响应类型-HTML格式

### HTML响应概述

FastAPI支持返回HTML格式的响应，适用于构建简单的Web页面或模板渲染。

### 基本用法

#### 直接返回HTML字符串

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    html_content = """
    <html>
        <head>
            <title>FastAPI HTML Response</title>
        </head>
        <body>
            <h1>Hello, FastAPI!</h1>
            <p>This is an HTML response.</p>
        </body>
    </html>
    """
    return HTMLResponse(content=html_content, status_code=200)
```

**浏览器访问方式：**
- 直接在浏览器地址栏输入 `http://127.0.0.1:8000/` → 看到HTML页面

### 使用HTMLResponse

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/", response_class=HTMLResponse)
def read_root():
    return """
    <html>
        <head>
            <title>Home Page</title>
        </head>
        <body>
            <h1>Welcome to FastAPI!</h1>
        </body>
    </html>
    """
```

**浏览器访问方式：**
- 直接在浏览器地址栏输入 `http://127.0.0.1:8000/` → 看到HTML页面

### 动态HTML内容

```python
@app.get("/user/{username}", response_class=HTMLResponse)
def get_user(username: str):
    return f"""
    <html>
        <head>
            <title>User Profile</title>
        </head>
        <body>
            <h1>Welcome, {username}!</h1>
            <p>This is your profile page.</p>
        </body>
    </html>
    """
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/user/John` → 看到"Welcome, John!"的HTML页面

### 使用模板引擎

#### 安装依赖

```bash
pip install jinja2
```

#### 配置模板

```python
from fastapi import FastAPI, Request
from fastapi.templating import Jinja2Templates

app = FastAPI()
templates = Jinja2Templates(directory="templates")

@app.get("/items/{item_id}", response_class=HTMLResponse)
async def read_item(request: Request, item_id: int):
    return templates.TemplateResponse(
        "item.html",
        {"request": request, "item_id": item_id}
    )
```

#### 创建模板文件

**templates/item.html**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Item Details</title>
</head>
<body>
    <h1>Item ID: {{ item_id }}</h1>
    <p>This is item number {{ item_id }}.</p>
</body>
</html>
```

### 表单处理

FastAPI支持处理HTML表单提交，使用`Form`类定义表单参数。完整的表单参数使用方法请参考第11.5章「表单参数」，包括：

- 基础表单参数定义
- 表单参数校验（长度、范围、正则表达式）
- 表单与文件混合上传
- HTML表单页面示例

### 本章小结

FastAPI支持HTML响应：

1. 使用`HTMLResponse`返回HTML内容
2. 可以直接返回HTML字符串
3. 支持Jinja2模板引擎
4. 支持表单处理和动态内容

下一章将学习表单参数。

---

## 11.5 FastAPI基础入门 - 表单参数

### 表单参数概述

表单参数（Form Parameters）是Web应用中常用的参数传递方式，用于处理HTML表单提交的数据。FastAPI使用`Form`类来定义表单参数。

### 安装依赖

处理表单数据需要安装`python-multipart`包：

```bash
pip install python-multipart
```

### 导入Form

```python
from fastapi import FastAPI, Form
```

### 基础表单参数

#### 单字段表单

```python
from fastapi import FastAPI, Form

app = FastAPI()

@app.post("/login/")
async def login(username: str = Form(...), password: str = Form(...)):
    return {"username": username, "password": "******"}
```

**浏览器访问方式**：
- 使用curl提交表单：`curl -X POST "http://127.0.0.1:8000/login/" -d "username=admin&password=123456"`
- 使用Swagger文档：打开`http://127.0.0.1:8000/docs`，找到`/login/`接口，点击"Try it out"，填写表单后点击"Execute"

### 多字段表单

#### 完整用户注册表单

```python
@app.post("/register/")
async def register(
    username: str = Form(..., description="用户名"),
    email: str = Form(..., description="邮箱"),
    password: str = Form(..., description="密码"),
    confirm_password: str = Form(..., description="确认密码"),
    age: int = Form(None, description="年龄"),
    gender: str = Form(None, description="性别")
):
    if password != confirm_password:
        raise HTTPException(status_code=400, detail="两次密码不一致")
    
    return {
        "username": username,
        "email": email,
        "age": age,
        "gender": gender
    }
```

**浏览器访问方式**：
- 使用curl：`curl -X POST "http://127.0.0.1:8000/register/" -d "username=john&email=john@example.com&password=pass123&confirm_password=pass123&age=25"`

### 表单参数校验

#### 长度和范围验证

```python
@app.post("/submit/")
async def submit_form(
    name: str = Form(..., min_length=1, max_length=50, description="姓名"),
    age: int = Form(None, ge=1, le=120, description="年龄"),
    phone: str = Form(..., pattern="^1[3-9]\d{9}$", description="手机号")
):
    return {"name": name, "age": age, "phone": phone}
```

**浏览器访问方式**：
- 有效请求：`curl -X POST "http://127.0.0.1:8000/submit/" -d "name=John&age=30&phone=13812345678"`
- 无效请求（手机号错误）：`curl -X POST "http://127.0.0.1:8000/submit/" -d "name=John&phone=12345"` → 返回422验证错误

### 表单参数完整分类

#### 一、第一位置参数：默认值/必填标记

```python
username: str = Form(...)          # 必填参数
username: str = Form(None)         # 可选参数，默认值为None
username: str = Form("guest")      # 可选参数，默认值为"guest"
```

#### 二、校验属性（验证规则）—— 触发参数校验，非法值返回422

**数字类型适用（int/float）**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `gt` | greater than，大于 | `gt=0` |
| `ge` | greater equal，大于等于 | `ge=1` |
| `lt` | less than，小于 | `lt=100` |
| `le` | less equal，小于等于 | `le=120` |

**字符串适用**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `min_length` | 最小长度 | `min_length=3` |
| `max_length` | 最大长度 | `max_length=50` |
| `pattern` | 正则表达式 | `pattern="^1[3-9]\d{9}$"` |

#### 三、元信息属性 —— 只用于生成接口文档

| 参数 | 说明 | 示例 |
|------|------|------|
| `title` | 参数标题 | `title="Username"` |
| `description` | 参数详细描述 | `description="User login name"` |
| `example` | 单个示例值 | `example="admin"` |
| `examples` | 多个示例 | `examples={"valid": {"value": "user123"}}` |
| `deprecated` | 是否废弃 | `deprecated=True` |

#### 四、其他通用配置

| 参数 | 说明 | 示例 |
|------|------|------|
| `alias` | 参数别名 | `alias="user-name"` |
| `include_in_schema` | 是否在文档中显示 | `include_in_schema=False` |

### 表单与文件混合上传

#### 同时接收表单数据和文件

```python
from fastapi import FastAPI, UploadFile, File, Form

@app.post("/upload-with-form/")
async def upload_with_form(
    file: UploadFile = File(...),
    title: str = Form(...),
    description: str = Form(None),
    category: str = Form("default")
):
    return {
        "filename": file.filename,
        "title": title,
        "description": description,
        "category": category
    }
```

**浏览器访问方式**：
- 使用curl：`curl -X POST "http://127.0.0.1:8000/upload-with-form/" -F "file=@document.pdf" -F "title=重要文档" -F "category=reports"`

### HTML表单示例

#### 创建HTML表单页面

```python
from fastapi import FastAPI, Form, HTMLResponse

app = FastAPI()

@app.get("/", response_class=HTMLResponse)
async def get_form():
    return """
    <html>
        <body>
            <h1>用户登录</h1>
            <form action="/login/" method="post">
                <div>
                    <label>用户名：</label>
                    <input type="text" name="username" required>
                </div>
                <div>
                    <label>密码：</label>
                    <input type="password" name="password" required>
                </div>
                <div>
                    <button type="submit">登录</button>
                </div>
            </form>
        </body>
    </html>
    """

@app.post("/login/", response_class=HTMLResponse)
async def login(username: str = Form(...), password: str = Form(...)):
    if username == "admin" and password == "123456":
        return f"""
        <html>
            <body>
                <h1>登录成功！</h1>
                <p>欢迎回来，{username}！</p>
            </body>
        </html>
        """
    else:
        return """
        <html>
            <body>
                <h1>登录失败！</h1>
                <p>用户名或密码错误</p>
                <a href="/">返回登录页</a>
            </body>
        </html>
        """
```

**浏览器访问方式**：
- 打开 `http://127.0.0.1:8000/` → 看到登录表单页面
- 输入用户名`admin`和密码`123456` → 点击登录 → 显示"登录成功"
- 输入错误密码 → 显示"登录失败"

### 完整示例：用户注册API

```python
from fastapi import FastAPI, Form, HTTPException
from typing import Optional

app = FastAPI(title="用户注册API")

@app.post("/user/register", tags=["用户管理"])
async def register_user(
    username: str = Form(
        ...,
        min_length=3,
        max_length=50,
        pattern="^[a-zA-Z0-9_]+$",
        description="用户名，3-50个字符，只能包含字母、数字和下划线"
    ),
    email: str = Form(
        ...,
        pattern="^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$",
        description="邮箱地址"
    ),
    password: str = Form(
        ...,
        min_length=6,
        description="密码，至少6个字符"
    ),
    confirm_password: str = Form(
        ...,
        description="确认密码"
    ),
    age: Optional[int] = Form(
        None,
        ge=1,
        le=120,
        description="年龄"
    ),
    gender: Optional[str] = Form(
        None,
        description="性别"
    )
):
    if password != confirm_password:
        raise HTTPException(status_code=400, detail="两次密码不一致")
    
    if len(password) < 6:
        raise HTTPException(status_code=400, detail="密码长度至少6位")
    
    return {
        "message": "注册成功",
        "user": {
            "username": username,
            "email": email,
            "age": age,
            "gender": gender
        }
    }

@app.post("/user/login", tags=["用户管理"])
async def login_user(
    username: str = Form(..., description="用户名"),
    password: str = Form(..., description="密码")
):
    if username == "admin" and password == "123456":
        return {"message": "登录成功", "token": "fake-jwt-token"}
    else:
        raise HTTPException(status_code=401, detail="用户名或密码错误")

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

**启动命令**：
```bash
uvicorn form_app:app --reload
```

**浏览器访问方式汇总**：

| 操作 | 方法 | URL | 示例 |
|------|------|-----|------|
| 用户注册 | POST | `/user/register` | `curl -X POST "http://127.0.0.1:8000/user/register" -d "username=john&email=john@example.com&password=pass123&confirm_password=pass123"` |
| 用户登录 | POST | `/user/login` | `curl -X POST "http://127.0.0.1:8000/user/login" -d "username=admin&password=123456"` |
| HTML表单 | GET | `/` | 直接在浏览器中打开 `http://127.0.0.1:8000/` |

### 本章小结

表单参数是Web开发的重要功能：

1. **使用`Form`类定义表单参数**
2. **必须使用POST请求**提交表单数据
3. 需要安装`python-multipart`依赖
4. 支持参数校验（长度、范围、正则表达式）
5. 支持元信息（title、description、example）
6. 可以与文件上传混合使用

---

## 11.6 FastAPI基础入门 - 文件上传

### 文件上传概述

文件上传是Web应用中常用的功能，FastAPI使用`UploadFile`类来处理文件上传。**文件上传必须使用POST请求**，因为GET请求无法携带文件数据。

### 安装依赖

处理文件上传和表单数据需要安装`python-multipart`包：

```bash
pip install python-multipart
```

### 导入UploadFile

```python
from fastapi import FastAPI, UploadFile, File
```

### 单文件上传

#### 基本用法

```python
from fastapi import FastAPI, UploadFile, File

app = FastAPI()

@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile = File(...)):
    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "file_size": len(await file.read())
    }
```

**浏览器访问方式**（使用curl或Swagger文档）：
- 使用curl上传文件：`curl -X POST "http://127.0.0.1:8000/uploadfile/" -F "file=@path/to/your/file.pdf"`
- 使用Swagger文档：打开`http://127.0.0.1:8000/docs`，找到`/uploadfile/`接口，点击"Try it out"，选择文件后点击"Execute"

#### UploadFile属性

| 属性 | 说明 | 类型 |
|------|------|------|
| `filename` | 上传文件的原始文件名 | str |
| `content_type` | 文件的MIME类型 | str |
| `file` | 文件对象本身（类似Python的file对象） | SpooledTemporaryFile |

#### UploadFile方法

| 方法 | 说明 | 返回值 |
|------|------|--------|
| `read(size=-1)` | 读取文件内容 | bytes |
| `write(data)` | 写入数据到文件 | int |
| `seek(offset)` | 移动文件指针 | int |
| `close()` | 关闭文件 | None |

### 多文件上传

#### 基本用法

```python
@app.post("/uploadfiles/")
async def create_upload_files(files: list[UploadFile] = File(...)):
    return [
        {"filename": file.filename, "content_type": file.content_type}
        for file in files
    ]
```

**浏览器访问方式**：
- 使用curl上传多个文件：`curl -X POST "http://127.0.0.1:8000/uploadfiles/" -F "files=@file1.pdf" -F "files=@file2.txt"`

### 带参数的文件上传

#### 文件上传时携带额外参数

```python
from fastapi import FastAPI, UploadFile, File, Form

@app.post("/upload-with-info/")
async def upload_with_info(
    file: UploadFile = File(...),
    description: str = Form(None),
    category: str = Form(...)
):
    return {
        "filename": file.filename,
        "description": description,
        "category": category
    }
```

**浏览器访问方式**：
- 使用curl：`curl -X POST "http://127.0.0.1:8000/upload-with-info/" -F "file=@document.pdf" -F "description=Important document" -F "category=reports"`

### 文件保存到服务器

#### 保存上传的文件

```python
import os

@app.post("/save-file/")
async def save_file(file: UploadFile = File(...)):
    os.makedirs("uploads", exist_ok=True)
    
    file_path = f"uploads/{file.filename}"
    with open(file_path, "wb") as buffer:
        buffer.write(await file.read())
    
    return {"message": f"文件已保存到 {file_path}"}
```

**浏览器访问方式**：
- 使用curl：`curl -X POST "http://127.0.0.1:8000/save-file/" -F "file=@yourfile.txt"`

### 文件大小限制

#### 使用常量配置

```python
MAX_FILE_SIZE = 10 * 1024 * 1024  # 10MB

@app.post("/upload-large/")
async def upload_large_file(file: UploadFile = File(..., description=f"最大{MAX_FILE_SIZE // (1024*1024)}MB")):
    content = await file.read()
    if len(content) > MAX_FILE_SIZE:
        raise HTTPException(status_code=413, detail=f"文件大小超过限制，最大允许{MAX_FILE_SIZE // (1024*1024)}MB")
    
    return {"filename": file.filename, "size": len(content)}
```

**浏览器访问方式**：
- 使用curl：`curl -X POST "http://127.0.0.1:8000/upload-large/" -F "file=@large_file.pdf"`

### 文件类型限制

#### 验证文件类型

```python
from fastapi import HTTPException

ALLOWED_EXTENSIONS = {"pdf", "txt", "png", "jpg", "jpeg"}

def allowed_file(filename: str):
    return "." in filename and \
           filename.rsplit(".", 1)[1].lower() in ALLOWED_EXTENSIONS

@app.post("/upload-restricted/")
async def upload_restricted(file: UploadFile = File(...)):
    if not allowed_file(file.filename):
        raise HTTPException(
            status_code=400,
            detail=f"不支持的文件类型，仅支持: {', '.join(ALLOWED_EXTENSIONS)}"
        )
    
    return {"filename": file.filename, "message": "文件类型验证通过"}
```

**浏览器访问方式**：
- 使用curl：`curl -X POST "http://127.0.0.1:8000/upload-restricted/" -F "file=@image.png"`

### 完整示例：文件上传API

```python
from fastapi import FastAPI, UploadFile, File, Form, HTTPException
import os
from typing import List

app = FastAPI(title="文件上传API")

UPLOAD_DIR = "uploads"
os.makedirs(UPLOAD_DIR, exist_ok=True)

ALLOWED_EXTENSIONS = {"pdf", "txt", "png", "jpg", "jpeg", "csv"}

def allowed_file(filename: str) -> bool:
    return "." in filename and \
           filename.rsplit(".", 1)[1].lower() in ALLOWED_EXTENSIONS

@app.post("/upload/single", tags=["文件上传"])
async def upload_single_file(
    file: UploadFile = File(...),
    description: str = Form(None)
):
    if not allowed_file(file.filename):
        raise HTTPException(
            status_code=400,
            detail=f"不支持的文件类型"
        )
    
    file_path = os.path.join(UPLOAD_DIR, file.filename)
    with open(file_path, "wb") as buffer:
        buffer.write(await file.read())
    
    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "path": file_path,
        "description": description
    }

@app.post("/upload/multiple", tags=["文件上传"])
async def upload_multiple_files(files: List[UploadFile] = File(...)):
    results = []
    for file in files:
        if not allowed_file(file.filename):
            results.append({"filename": file.filename, "error": "不支持的文件类型"})
            continue
        
        file_path = os.path.join(UPLOAD_DIR, file.filename)
        with open(file_path, "wb") as buffer:
            buffer.write(await file.read())
        
        results.append({
            "filename": file.filename,
            "content_type": file.content_type,
            "path": file_path
        })
    
    return {"files": results}

@app.get("/upload/files", tags=["文件管理"])
async def list_uploaded_files():
    files = []
    for filename in os.listdir(UPLOAD_DIR):
        filepath = os.path.join(UPLOAD_DIR, filename)
        if os.path.isfile(filepath):
            files.append({
                "filename": filename,
                "size": os.path.getsize(filepath),
                "path": filepath
            })
    return {"files": files}

@app.delete("/upload/files/{filename}", tags=["文件管理"])
async def delete_uploaded_file(filename: str):
    file_path = os.path.join(UPLOAD_DIR, filename)
    if not os.path.exists(file_path):
        raise HTTPException(status_code=404, detail="文件不存在")
    
    os.remove(file_path)
    return {"message": f"文件 {filename} 已删除"}

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

**启动命令**：
```bash
uvicorn file_upload_app:app --reload
```

**浏览器访问方式汇总**：

| 操作 | 方法 | URL | 示例 |
|------|------|-----|------|
| 单文件上传 | POST | `/upload/single` | `curl -X POST "http://127.0.0.1:8000/upload/single" -F "file=@document.pdf"` |
| 多文件上传 | POST | `/upload/multiple` | `curl -X POST "http://127.0.0.1:8000/upload/multiple" -F "files=@file1.pdf" -F "files=@file2.txt"` |
| 查看文件列表 | GET | `/upload/files` | `http://127.0.0.1:8000/upload/files` |
| 删除文件 | DELETE | `/upload/files/{filename}` | `curl -X DELETE "http://127.0.0.1:8000/upload/files/document.pdf"` |

### 本章小结

文件上传是Web开发的重要功能：

1. **必须使用POST请求**：GET请求无法携带文件数据
2. 使用`UploadFile`类处理上传文件
3. `File`参数用于声明文件上传字段
4. `Form`参数用于同时接收表单数据
5. 支持单文件和多文件上传
6. 需要进行文件类型和大小验证

---

## 12. FastAPI基础入门 - 响应类型-文件格式

### 文件响应概述

FastAPI支持返回文件格式的响应，包括普通文件、图片、二进制数据等。

### FileResponse

#### 基本用法

```python
from fastapi import FastAPI
from fastapi.responses import FileResponse

app = FastAPI()

@app.get("/files/{file_path:path}")
async def read_file(file_path: str):
    return FileResponse(file_path)
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/files/document.pdf` → 浏览器下载或预览文件

#### 指定文件名

```python
@app.get("/download/")
async def download_file():
    file_path = "data/example.pdf"
    return FileResponse(
        path=file_path,
        filename="downloaded.pdf",
        media_type="application/pdf"
    )
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/download/` → 浏览器下载文件（文件名为downloaded.pdf）

### StreamingResponse

#### 流式响应

```python
from fastapi.responses import StreamingResponse
import io

@app.get("/stream/")
async def stream_file():
    def generate():
        with open("large_file.txt", "rb") as f:
            while chunk := f.read(1024):
                yield chunk
    return StreamingResponse(generate())
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/stream/` → 浏览器流式接收文件内容

#### 动态生成内容

```python
from fastapi.responses import StreamingResponse
import csv
from io import StringIO

@app.get("/csv/")
async def get_csv():
    def generate_csv():
        output = StringIO()
        writer = csv.writer(output)
        writer.writerow(["Name", "Age", "Email"])
        writer.writerow(["John", "30", "john@example.com"])
        writer.writerow(["Jane", "25", "jane@example.com"])
        yield output.getvalue()
    
    return StreamingResponse(
        generate_csv(),
        media_type="text/csv",
        headers={"Content-Disposition": "attachment; filename=data.csv"}
    )
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/csv/` → 浏览器下载CSV文件（文件名为data.csv）

### JSONResponse

#### 自定义JSON响应

```python
from fastapi.responses import JSONResponse

@app.get("/custom-json/")
async def custom_json():
    data = {"message": "Hello", "status": "success"}
    return JSONResponse(
        content=data,
        status_code=200,
        headers={"X-Custom-Header": "custom-value"}
    )
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/custom-json/` → `{"message": "Hello", "status": "success"}`

### RedirectResponse

#### 重定向

```python
from fastapi.responses import RedirectResponse

@app.get("/redirect/")
async def redirect():
    return RedirectResponse(url="https://fastapi.tiangolo.com")
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/redirect/` → 浏览器自动跳转到FastAPI官网

#### 带状态码的重定向

```python
@app.post("/submit/")
async def submit():
    # 处理提交逻辑
    return RedirectResponse(url="/success/", status_code=303)
```

### PlainTextResponse

#### 纯文本响应

```python
from fastapi.responses import PlainTextResponse

@app.get("/text/")
async def get_text():
    return PlainTextResponse(content="Hello, World!", status_code=200)
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/text/` → 浏览器显示纯文本"Hello, World!"

### 本章小结

FastAPI提供多种文件响应类型：

1. `FileResponse`：返回文件
2. `StreamingResponse`：流式响应
3. `JSONResponse`：自定义JSON响应
4. `RedirectResponse`：重定向
5. `PlainTextResponse`：纯文本响应

下一章将学习自定义响应数据格式。

---

## 13. FastAPI基础入门 - 自定义响应数据格式

### 自定义响应概述

FastAPI允许自定义响应数据格式，以满足特定的业务需求。

### 统一响应格式

#### 创建通用响应模型

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import Generic, TypeVar, Optional

app = FastAPI()

T = TypeVar('T')

class ResponseModel(BaseModel, Generic[T]):
    code: int = 200
    message: str = "success"
    data: Optional[T] = None
```

#### 使用统一响应格式

```python
class Item(BaseModel):
    name: str
    price: float

@app.get("/items/{item_id}", response_model=ResponseModel[Item])
def get_item(item_id: int):
    item_data = {"name": "Foo", "price": 35.4}
    return {"code": 200, "message": "success", "data": item_data}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/1` → `{"code": 200, "message": "success", "data": {"name": "Foo", "price": 35.4}}`

### 自定义响应类

#### 创建自定义响应

```python
from fastapi.responses import Response
import json

class CustomResponse(Response):
    media_type = "application/json"
    
    def __init__(self, content=None, status_code=200, headers=None):
        if content is None:
            content = {}
        
        # 添加统一包装
        wrapped_content = {
            "code": status_code,
            "message": "success" if 200 <= status_code < 300 else "error",
            "data": content
        }
        
        super().__init__(
            content=json.dumps(wrapped_content),
            status_code=status_code,
            headers=headers,
            media_type=self.media_type
        )

```python
@app.get("/custom/{item_id}")
def get_custom(item_id: int):
    return CustomResponse(content={"item_id": item_id, "name": "Foo"})
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/custom/1` → `{"code": 200, "message": "success", "data": {"item_id": 1, "name": "Foo"}}`

### 中间件统一包装

#### 创建响应包装中间件

```python
from fastapi import FastAPI, Request
from starlette.middleware.base import BaseHTTPMiddleware
from starlette.responses import JSONResponse
import json

app = FastAPI()

class ResponseMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        response = await call_next(request)
        
        # 读取原始响应内容
        body = b""
        async for chunk in response.body_iterator:
            body += chunk
        
        # 解析原始内容
        try:
            data = json.loads(body.decode())
        except:
            data = body.decode()
        
        # 包装响应
        wrapped_data = {
            "code": response.status_code,
            "message": "success" if 200 <= response.status_code < 300 else "error",
            "data": data
        }
        
        return JSONResponse(content=wrapped_data, status_code=response.status_code)

app.add_middleware(ResponseMiddleware)

```python
@app.get("/items/{item_id}")
def get_item(item_id: int):
    return {"item_id": item_id, "name": "Foo"}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/1` → `{"code": 200, "message": "success", "data": {"item_id": 1, "name": "Foo"}}`

### 自定义媒体类型

```python
from fastapi.responses import Response

@app.get("/xml/")
def get_xml():
    xml_content = """<?xml version="1.0" encoding="UTF-8"?>
    <response>
        <code>200</code>
        <message>success</message>
        <data>
            <item>
                <id>1</id>
                <name>Foo</name>
            </item>
        </data>
    </response>
    """
    return Response(content=xml_content, media_type="application/xml")
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/xml/` → 浏览器显示XML格式响应

### 本章小结

自定义响应格式可以统一API返回格式：

1. 使用泛型模型创建统一响应结构
2. 创建自定义响应类
3. 使用中间件统一包装所有响应
4. 支持自定义媒体类型

下一章将学习异常响应处理。

---

## 14. FastAPI基础入门 - 异常响应处理

### 异常处理概述

FastAPI提供了多种方式来处理异常和返回自定义错误响应。

### HTTPException

#### 基本用法

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

items = {"foo": "The Foo Wrestlers"}

@app.get("/items/{item_id}")
def read_item(item_id: str):
    if item_id not in items:
        raise HTTPException(
            status_code=404,
            detail="Item not found",
            headers={"X-Error": "Item not found"}
        )
    return {"item": items[item_id]}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/foo` → `{"item": "The Foo Wrestlers"}`
- `http://127.0.0.1:8000/items/bar` → 返回404错误 `{"detail": "Item not found"}`

#### 自定义异常类

```python
from fastapi import HTTPException

class ItemNotFoundError(HTTPException):
    def __init__(self, item_id: str):
        super().__init__(
            status_code=404,
            detail=f"Item '{item_id}' not found",
            headers={"X-Error": "ItemNotFound"}
        )

@app.get("/items/{item_id}")
def read_item(item_id: str):
    if item_id not in items:
        raise ItemNotFoundError(item_id)
    return {"item": items[item_id]}
```

### RequestValidationError

#### 自定义验证错误处理

```python
from fastapi import FastAPI, Request, status
from fastapi.encoders import jsonable_encoder
from fastapi.exceptions import RequestValidationError
from fastapi.responses import JSONResponse

app = FastAPI()

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request: Request, exc: RequestValidationError):
    return JSONResponse(
        status_code=status.HTTP_422_UNPROCESSABLE_ENTITY,
        content=jsonable_encoder({
            "code": 422,
            "message": "Validation error",
            "details": exc.errors()
        })
    )

@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}
```

### 全局异常处理

#### 捕获所有异常

```python
from fastapi import FastAPI, Request, status
from fastapi.responses import JSONResponse
import traceback

app = FastAPI()

@app.exception_handler(Exception)
async def global_exception_handler(request: Request, exc: Exception):
    # 记录异常信息
    error_trace = traceback.format_exc()
    print(f"Error: {error_trace}")
    
    return JSONResponse(
        status_code=status.HTTP_500_INTERNAL_SERVER_ERROR,
        content={
            "code": 500,
            "message": "Internal server error",
            "detail": "Something went wrong"
        }
    )

```python
@app.get("/error/")
def trigger_error():
    raise ValueError("Something went wrong!")
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/error/` → 返回500错误 `{"code": 500, "message": "Internal server error", "detail": "Something went wrong"}`

### 自定义异常处理器

```python
class CustomError(Exception):
    def __init__(self, code: int, message: str):
        self.code = code
        self.message = message

@app.exception_handler(CustomError)
async def custom_error_handler(request: Request, exc: CustomError):
    return JSONResponse(
        status_code=exc.code,
        content={
            "code": exc.code,
            "message": exc.message
        }
    )

```python
@app.get("/custom-error/")
def trigger_custom_error():
    raise CustomError(code=400, message="Custom error message")
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/custom-error/` → 返回400错误 `{"code": 400, "message": "Custom error message"}`

### 本章小结

异常处理是构建健壮API的重要部分：

1. 使用`HTTPException`抛出HTTP错误
2. 自定义验证错误处理器
3. 捕获全局异常
4. 创建自定义异常类和处理器

下一章将学习中间件的概念和用法。

---

## 15. FastAPI进阶 - 中间件

### 中间件概述

中间件是在请求处理前后执行的代码，用于处理跨切面关注点。

### 内置中间件

#### CORS中间件

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

origins = [
    "http://localhost:8000",
    "https://example.com"
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

@app.get("/")
def read_root():
    return {"message": "Hello, World!"}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/` → `{"message": "Hello, World!"}`

#### GZip中间件

```python
from fastapi.middleware.gzip import GZipMiddleware

app.add_middleware(GZipMiddleware, minimum_size=1000)
```

### 自定义中间件

#### 基本结构

```python
from fastapi import FastAPI, Request
from starlette.middleware.base import BaseHTTPMiddleware

app = FastAPI()

class CustomMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        # 请求处理前
        print(f"Request path: {request.url.path}")
        
        # 调用下一个中间件或路由处理函数
        response = await call_next(request)
        
        # 请求处理后
        print(f"Response status: {response.status_code}")
        
        return response

app.add_middleware(CustomMiddleware)

```python
@app.get("/")
def read_root():
    return {"message": "Hello, World!"}
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/` → `{"message": "Hello, World!"}`

#### 计时中间件

```python
import time

class TimingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        start_time = time.time()
        response = await call_next(request)
        duration = time.time() - start_time
        response.headers["X-Response-Time"] = f"{duration:.2f}s"
        return response

app.add_middleware(TimingMiddleware)
```

#### 认证中间件

```python
class AuthMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        token = request.headers.get("Authorization")
        
        if not token:
            from fastapi.responses import JSONResponse
            return JSONResponse(
                status_code=401,
                content={"message": "Unauthorized"}
            )
        
        # 验证token逻辑
        if token != "Bearer secret-token":
            from fastapi.responses import JSONResponse
            return JSONResponse(
                status_code=403,
                content={"message": "Forbidden"}
            )
        
        return await call_next(request)

app.add_middleware(AuthMiddleware)
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/` → 返回401错误 `{"message": "Unauthorized"}`
- 使用curl携带正确token：`curl -H "Authorization: Bearer secret-token" http://127.0.0.1:8000/` → `{"message": "Hello, World!"}`

### 多个中间件的执行顺序

```python
app.add_middleware(Middleware1)  # 第一个执行
app.add_middleware(Middleware2)  # 第二个执行
app.add_middleware(Middleware3)  # 第三个执行

# 请求处理顺序:
# Middleware1 → Middleware2 → Middleware3 → 路由处理函数
# 响应处理顺序:
# Middleware3 → Middleware2 → Middleware1
```

### 本章小结

中间件是处理请求的重要机制：

1. 使用`add_middleware`添加中间件
2. FastAPI内置了CORS、GZip等中间件
3. 可以创建自定义中间件
4. 中间件按添加顺序执行

下一章将学习依赖注入。

---

## 16. FastAPI进阶 - 依赖注入

### 依赖注入概述

依赖注入是一种设计模式，用于解耦组件之间的依赖关系。

### 基本用法

#### 创建依赖函数

```python
from fastapi import FastAPI, Depends

app = FastAPI()

async def common_parameters(q: str = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}

@app.get("/items/")
async def read_items(commons: dict = Depends(common_parameters)):
    return commons

@app.get("/users/")
async def read_users(commons: dict = Depends(common_parameters)):
    return commons
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/` → `{"q": null, "skip": 0, "limit": 100}`
- `http://127.0.0.1:8000/items/?q=hello&skip=10&limit=20` → `{"q": "hello", "skip": 10, "limit": 20}`
- `http://127.0.0.1:8000/users/?q=john` → `{"q": "john", "skip": 0, "limit": 100}`

#### 类作为依赖

```python
from typing import Optional

class CommonQueryParams:
    def __init__(self, q: Optional[str] = None, skip: int = 0, limit: int = 100):
        self.q = q
        self.skip = skip
        self.limit = limit

@app.get("/items/")
async def read_items(commons: CommonQueryParams = Depends()):
    return {"q": commons.q, "skip": commons.skip, "limit": commons.limit}
```

### 依赖链

#### 依赖其他依赖

```python
async def get_db():
    db = "database connection"
    yield db
    # 清理资源

async def get_user(db: str = Depends(get_db)):
    return {"user": "john", "db": db}

```python
@app.get("/users/me")
async def read_user(user: dict = Depends(get_user)):
    return user
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/users/me` → `{"user": "john", "db": "database connection"}`

### 子依赖

#### 可复用的依赖

```python
from fastapi import Depends, FastAPI, Header, HTTPException

app = FastAPI()

async def verify_token(x_token: str = Header(...)):
    if x_token != "fake-super-secret-token":
        raise HTTPException(status_code=400, detail="X-Token header invalid")

async def verify_key(x_key: str = Header(...)):
    if x_key != "fake-super-secret-key":
        raise HTTPException(status_code=400, detail="X-Key header invalid")
    return x_key

```python
@app.get("/items/", dependencies=[Depends(verify_token), Depends(verify_key)])
async def read_items():
    return [{"item": "Foo"}, {"item": "Bar"}]
```

**浏览器访问方式：**
- 使用curl携带正确header：`curl -H "X-Token: fake-super-secret-token" -H "X-Key: fake-super-secret-key" http://127.0.0.1:8000/items/` → `[{"item": "Foo"}, {"item": "Bar"}]`

### 全局依赖

#### 应用级依赖

```python
async def global_dependency(request: Request):
    # 全局处理逻辑
    pass

app = FastAPI(dependencies=[Depends(global_dependency)])
```

### 带yield的依赖

#### 资源管理

```python
async def get_db_connection():
    db = await create_db_connection()
    try:
        yield db
    finally:
        await db.close()

@app.get("/items/")
async def read_items(db = Depends(get_db_connection)):
    # 使用db连接
    pass
```

### 本章小结

依赖注入是FastAPI的强大特性：

1. 使用`Depends()`声明依赖
2. 支持函数和类作为依赖
3. 支持依赖链和子依赖
4. 支持资源管理（yield模式）

下一章将学习ORM简介及安装。

---

## 17. FastAPI进阶 - ORM简介及安装

### ORM概述

ORM（Object-Relational Mapping）是一种将对象模型与关系数据库进行映射的技术。

#### ORM的优势

1. **简化数据库操作**：使用面向对象的方式操作数据库
2. **跨数据库兼容**：一套代码支持多种数据库
3. **减少SQL编写**：自动生成SQL语句
4. **类型安全**：编译时类型检查

#### SQLAlchemy简介

SQLAlchemy是Python中最流行的ORM库，支持多种数据库后端。

### 安装依赖

```bash
pip install "sqlalchemy>=2.0.0" "aiomysql>=0.2.0"
```

#### 依赖说明

| 包 | 说明 |
|----|------|
| `sqlalchemy>=2.0.0` | SQLAlchemy核心库（2026年主流版本） |
| `aiomysql>=0.2.0` | MySQL异步驱动 |

#### 其他数据库驱动

```bash
# PostgreSQL（推荐）
pip install "asyncpg>=0.29.0"

# SQLite（内置支持，无需额外安装，推荐用于开发测试）
```

### 验证安装

```python
import sqlalchemy
from sqlalchemy.ext.asyncio import create_async_engine

print(f"SQLAlchemy version: {sqlalchemy.__version__}")
```

**运行方式：**
- 在Python文件中运行此代码，或在命令行中执行 `python -c "import sqlalchemy; print(sqlalchemy.__version__)"`

### 本章小结

ORM是现代Web开发的重要工具：

1. SQLAlchemy是Python最流行的ORM库
2. 安装命令：`pip install sqlalchemy[asyncio] aiomysql`
3. 支持多种数据库后端
4. 异步模式提供更好的性能

下一章将学习如何使用ORM创建数据库表。

---

## 18. FastAPI进阶 - ORM-建表

### 创建数据库表

#### 配置数据库连接

```python
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import declarative_base, sessionmaker

# MySQL连接配置
DATABASE_URL = "mysql+aiomysql://user:password@localhost/dbname"

# 创建异步引擎
engine = create_async_engine(DATABASE_URL, echo=True)

# 创建会话工厂
async_session = sessionmaker(engine, class_=AsyncSession, expire_on_commit=False)

# 创建基类
Base = declarative_base()
```

#### 定义模型

```python
from sqlalchemy import Column, Integer, String, Float, DateTime
from datetime import datetime

class Item(Base):
    __tablename__ = "items"
    
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String(100), index=True)
    description = Column(String(500), nullable=True)
    price = Column(Float)
    created_at = Column(DateTime, default=datetime.utcnow)
```

#### 更多模型示例

```python
class User(Base):
    __tablename__ = "users"
    
    id = Column(Integer, primary_key=True, index=True)
    username = Column(String(50), unique=True, index=True)
    email = Column(String(100), unique=True, index=True)
    hashed_password = Column(String(255))
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime, default=datetime.utcnow)

class Order(Base):
    __tablename__ = "orders"
    
    id = Column(Integer, primary_key=True, index=True)
    user_id = Column(Integer, ForeignKey("users.id"))
    item_id = Column(Integer, ForeignKey("items.id"))
    quantity = Column(Integer, default=1)
    total_price = Column(Float)
    created_at = Column(DateTime, default=datetime.utcnow)
```

#### 创建表

```python
async def create_tables():
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)

# 在应用启动时调用
import asyncio
asyncio.run(create_tables())
```

**运行方式：**
- 在Python文件中运行此代码，会自动在数据库中创建表

#### 模型字段类型

| SQLAlchemy类型 | Python类型 | SQL类型 |
|----------------|------------|---------|
| `Integer` | int | INTEGER |
| `String` | str | VARCHAR |
| `Float` | float | FLOAT |
| `Boolean` | bool | BOOLEAN |
| `DateTime` | datetime | DATETIME |
| `Date` | date | DATE |
| `Text` | str | TEXT |

#### 字段参数

| 参数 | 说明 |
|------|------|
| `primary_key` | 主键 |
| `index` | 索引 |
| `unique` | 唯一约束 |
| `nullable` | 是否允许为空 |
| `default` | 默认值 |
| `autoincrement` | 自增 |

### 本章小结

使用SQLAlchemy创建数据库表：

1. 配置数据库连接URL
2. 创建异步引擎和会话
3. 定义模型类继承自Base
4. 使用`create_all`创建表

下一章将学习如何在路由中使用ORM。

---

## 19. FastAPI进阶 - ORM-在路由中使用ORM

### 在FastAPI路由中集成ORM

#### 创建数据库依赖

```python
from fastapi import FastAPI, Depends
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import declarative_base, sessionmaker

DATABASE_URL = "mysql+aiomysql://user:password@localhost/dbname"

engine = create_async_engine(DATABASE_URL)
async_session = sessionmaker(engine, class_=AsyncSession, expire_on_commit=False)
Base = declarative_base()

# 数据库依赖
async def get_db():
    async with async_session() as session:
        yield session

app = FastAPI()
```

#### 创建模型

```python
from sqlalchemy import Column, Integer, String, Float, DateTime
from datetime import datetime

class Item(Base):
    __tablename__ = "items"
    
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String(100), index=True)
    description = Column(String(500), nullable=True)
    price = Column(Float)
    created_at = Column(DateTime, default=datetime.utcnow)
```

#### 创建Pydantic模型

```python
from pydantic import BaseModel
from datetime import datetime

class ItemCreate(BaseModel):
    name: str
    description: str = None
    price: float

class ItemResponse(BaseModel):
    id: int
    name: str
    description: str = None
    price: float
    created_at: datetime
    
    class Config:
        orm_mode = True
```

#### 在路由中使用ORM

```python
from sqlalchemy.future import select

@app.get("/items/", response_model=list[ItemResponse])
async def read_items(db: AsyncSession = Depends(get_db)):
    result = await db.execute(select(Item))
    items = result.scalars().all()
    return items

@app.get("/items/{item_id}", response_model=ItemResponse)
async def read_item(item_id: int, db: AsyncSession = Depends(get_db)):
    result = await db.execute(select(Item).where(Item.id == item_id))
    item = result.scalar_one_or_none()
    if item is None:
        raise HTTPException(status_code=404, detail="Item not found")
    return item

@app.post("/items/", response_model=ItemResponse)
async def create_item(item: ItemCreate, db: AsyncSession = Depends(get_db)):
    db_item = Item(**item.dict())
    db.add(db_item)
    await db.commit()
    await db.refresh(db_item)
    return db_item
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/` → 返回所有items列表（GET请求）
- `http://127.0.0.1:8000/items/1` → 返回id为1的item详情（GET请求）
- 使用curl创建item：`curl -X POST "http://127.0.0.1:8000/items/" -H "Content-Type: application/json" -d '{"name": "New Item", "price": 19.99}'`（POST请求）

### 完整示例

```python
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy.future import select
from sqlalchemy import Column, Integer, String, Float, DateTime
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import declarative_base, sessionmaker
from pydantic import BaseModel
from datetime import datetime

# 数据库配置
DATABASE_URL = "mysql+aiomysql://user:password@localhost/dbname"
engine = create_async_engine(DATABASE_URL)
async_session = sessionmaker(engine, class_=AsyncSession, expire_on_commit=False)
Base = declarative_base()

# 模型定义
class Item(Base):
    __tablename__ = "items"
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String(100), index=True)
    description = Column(String(500), nullable=True)
    price = Column(Float)
    created_at = Column(DateTime, default=datetime.utcnow)

# Pydantic模型
class ItemCreate(BaseModel):
    name: str
    description: str = None
    price: float

class ItemResponse(BaseModel):
    id: int
    name: str
    description: str = None
    price: float
    created_at: datetime
    class Config:
        orm_mode = True

# 依赖注入
async def get_db():
    async with async_session() as session:
        yield session

app = FastAPI()

# 路由
@app.get("/items/", response_model=list[ItemResponse])
async def read_items(db: AsyncSession = Depends(get_db)):
    result = await db.execute(select(Item))
    return result.scalars().all()
```

### 本章小结

在路由中使用ORM的步骤：

1. 创建数据库连接和会话
2. 定义SQLAlchemy模型
3. 定义Pydantic模型用于请求/响应
4. 使用依赖注入获取数据库会话
5. 在路由中执行数据库操作

下一章将学习ORM查询数据的方法。

---

## 20. FastAPI进阶 - ORM操作数据-查询数据

### 查询数据概述

SQLAlchemy提供了强大的查询API，可以灵活地从数据库中获取数据。

### 基础查询

#### 查询所有记录

```python
from sqlalchemy.future import select

async def get_all_items(db: AsyncSession):
    result = await db.execute(select(Item))
    items = result.scalars().all()
    return items
```

#### 查询单条记录

```python
async def get_item_by_id(db: AsyncSession, item_id: int):
    result = await db.execute(select(Item).where(Item.id == item_id))
    item = result.scalar_one_or_none()
    return item
```

#### 查询第一条记录

```python
async def get_first_item(db: AsyncSession):
    result = await db.execute(select(Item))
    item = result.scalar_first()
    return item
```

**浏览器访问方式**（假设将这些函数包装在路由中）：
- `http://127.0.0.1:8000/items/` → 返回所有items
- `http://127.0.0.1:8000/items/1` → 返回id为1的item

### 条件查询

#### 基本条件

```python
async def get_items_by_price(db: AsyncSession, min_price: float):
    result = await db.execute(select(Item).where(Item.price >= min_price))
    return result.scalars().all()
```

#### 多个条件

```python
async def get_items_by_price_range(db: AsyncSession, min_price: float, max_price: float):
    result = await db.execute(
        select(Item).where(Item.price >= min_price).where(Item.price <= max_price)
    )
    return result.scalars().all()
```

### 排序

```python
from sqlalchemy import desc, asc

async def get_items_sorted(db: AsyncSession, sort_by: str = "created_at"):
    if sort_by == "price":
        result = await db.execute(select(Item).order_by(asc(Item.price)))
    else:
        result = await db.execute(select(Item).order_by(desc(Item.created_at)))
    return result.scalars().all()
```

### 限制和偏移

```python
async def get_items_paged(db: AsyncSession, skip: int = 0, limit: int = 10):
    result = await db.execute(select(Item).offset(skip).limit(limit))
    return result.scalars().all()
```

**浏览器访问方式**（假设将这些函数包装在路由中）：
- `http://127.0.0.1:8000/items/?skip=0&limit=10` → 返回前10条items（分页查询）
- `http://127.0.0.1:8000/items/?sort_by=price` → 按价格排序返回items

### 本章小结

ORM查询的基本用法：

1. 使用`select()`构建查询
2. 使用`where()`添加条件
3. 使用`order_by()`排序
4. 使用`offset()`和`limit()`分页
5. 使用`scalars()`获取结果

下一章将学习条件查询的比较判断。

---

## 21. FastAPI进阶 - ORM操作数据-条件查询-比较判断

### 比较运算符

SQLAlchemy支持多种比较运算符用于条件查询。

#### 基本比较

```python
from sqlalchemy.future import select

# 等于
result = await db.execute(select(Item).where(Item.id == 1))

# 不等于
result = await db.execute(select(Item).where(Item.id != 1))

# 大于
result = await db.execute(select(Item).where(Item.price > 10))

# 大于等于
result = await db.execute(select(Item).where(Item.price >= 10))

# 小于
result = await db.execute(select(Item).where(Item.price < 100))

# 小于等于
result = await db.execute(select(Item).where(Item.price <= 100))
```

**浏览器访问方式**（假设将这些查询包装在路由中）：
- `http://127.0.0.1:8000/items/?price_min=10&price_max=100` → 返回价格在10-100之间的items

#### IN查询

```python
# IN查询
result = await db.execute(select(Item).where(Item.id.in_([1, 2, 3])))

# NOT IN查询
result = await db.execute(select(Item).where(~Item.id.in_([1, 2, 3])))
```

#### NULL判断

```python
# IS NULL
result = await db.execute(select(Item).where(Item.description.is_(None)))

# IS NOT NULL
result = await db.execute(select(Item).where(Item.description.is_not(None)))
```

#### 范围查询

```python
from sqlalchemy import between

# BETWEEN查询
result = await db.execute(select(Item).where(Item.price.between(10, 100)))

# 等价于
result = await db.execute(select(Item).where(Item.price >= 10).where(Item.price <= 100))
```

#### 组合条件

```python
from sqlalchemy import and_, or_, not_

# AND条件
result = await db.execute(
    select(Item).where(and_(Item.price > 10, Item.price < 100))
)

# OR条件
result = await db.execute(
    select(Item).where(or_(Item.name == "Foo", Item.name == "Bar"))
)

# NOT条件
result = await db.execute(select(Item).where(not_(Item.price < 10)))

# 混合条件
result = await db.execute(
    select(Item).where(
        and_(
            Item.price > 10,
            or_(Item.name == "Foo", Item.description.is_not(None))
        )
    )
)
```

### 本章小结

条件查询的比较运算符：

1. 基本比较：`==`, `!=`, `>`, `>=`, `<`, `<=`
2. IN查询：`in_()`
3. NULL判断：`is_()` 和 `is_not()`
4. 范围查询：`between()`
5. 逻辑组合：`and_()`, `or_()`, `not_()`

下一章将学习模糊查询和包含查询。

---

## 22. FastAPI进阶 - ORM操作数据-条件查询-模糊&与非&包含

### 模糊查询

#### LIKE查询

```python
from sqlalchemy.future import select

# 包含关键字
result = await db.execute(select(Item).where(Item.name.like("%Foo%")))

# 以关键字开头
result = await db.execute(select(Item).where(Item.name.like("Foo%")))

# 以关键字结尾
result = await db.execute(select(Item).where(Item.name.like("%Foo")))
```

**浏览器访问方式**（假设将这些查询包装在路由中）：
- `http://127.0.0.1:8000/items/?search=Foo` → 返回名称包含"Foo"的items（模糊查询）

#### ILIKE查询（不区分大小写）

```python
# 不区分大小写的模糊查询
result = await db.execute(select(Item).where(Item.name.ilike("%foo%")))
```

### 包含查询

#### 使用contains

```python
# 包含指定字符串
result = await db.execute(select(Item).where(Item.name.contains("Foo")))

# 不区分大小写
result = await db.execute(select(Item).where(Item.name.icontains("foo")))
```

#### 使用startswith和endswith

```python
# 以指定字符串开头
result = await db.execute(select(Item).where(Item.name.startswith("Foo")))

# 以指定字符串结尾
result = await db.execute(select(Item).where(Item.name.endswith("Bar")))

# 不区分大小写版本
result = await db.execute(select(Item).where(Item.name.istartswith("foo")))
result = await db.execute(select(Item).where(Item.name.iendswith("bar")))
```

### 正则表达式

```python
# 使用正则表达式
result = await db.execute(select(Item).where(Item.name.regexp_match(r'^[A-Z]')))

# 不区分大小写的正则匹配（MySQL）
result = await db.execute(select(Item).where(Item.name.regexp_match(r'^foo', flags='i')))
```

### 本章小结

模糊查询和包含查询的用法：

1. `like()` - SQL LIKE模式匹配
2. `ilike()` - 不区分大小写的LIKE
3. `contains()` - 字符串包含
4. `startswith()` / `endswith()` - 前缀/后缀匹配
5. `regexp_match()` - 正则表达式匹配

下一章将学习聚合查询。

---

## 23. FastAPI进阶 - ORM操作数据-聚合查询

### 聚合函数

SQLAlchemy提供了常用的聚合函数用于统计分析。

#### 导入聚合函数

```python
from sqlalchemy import func
```

#### COUNT

```python
from sqlalchemy.future import select

# 统计总记录数
result = await db.execute(select(func.count(Item.id)))
total = result.scalar()

# 统计非空字段数
result = await db.execute(select(func.count(Item.description)))
count = result.scalar()

# 统计不同值的数量
result = await db.execute(select(func.count(func.distinct(Item.category))))
distinct_count = result.scalar()
```

**浏览器访问方式**（假设将这些查询包装在路由中）：
- `http://127.0.0.1:8000/items/stats/` → 返回items的统计信息（总数、平均价格等）

#### SUM

```python
# 求和
result = await db.execute(select(func.sum(Item.price)))
total_price = result.scalar()

# 条件求和
result = await db.execute(
    select(func.sum(Item.price)).where(Item.category == "Electronics")
)
category_total = result.scalar()
```

#### AVG

```python
# 平均值
result = await db.execute(select(func.avg(Item.price)))
avg_price = result.scalar()
```

#### MIN和MAX

```python
# 最小值
result = await db.execute(select(func.min(Item.price)))
min_price = result.scalar()

# 最大值
result = await db.execute(select(func.max(Item.price)))
max_price = result.scalar()
```

#### 组合聚合

```python
# 同时查询多个聚合值
result = await db.execute(
    select(
        func.count(Item.id).label("total"),
        func.sum(Item.price).label("sum_price"),
        func.avg(Item.price).label("avg_price"),
        func.min(Item.price).label("min_price"),
        func.max(Item.price).label("max_price")
    )
)
stats = result.first()
```

### 分组聚合

#### GROUP BY

```python
# 按类别分组统计
result = await db.execute(
    select(
        Item.category,
        func.count(Item.id).label("count"),
        func.sum(Item.price).label("total")
    ).group_by(Item.category)
)
groups = result.all()
```

#### HAVING子句

```python
# 分组后筛选
result = await db.execute(
    select(
        Item.category,
        func.count(Item.id).label("count")
    )
    .group_by(Item.category)
    .having(func.count(Item.id) > 10)
)
groups = result.all()
```

### 本章小结

聚合查询的用法：

1. 使用`func`模块调用聚合函数
2. 常用聚合函数：`count`, `sum`, `avg`, `min`, `max`
3. 使用`group_by`进行分组
4. 使用`having`筛选分组结果

下一章将学习分页查询。

---

## 24. FastAPI进阶 - ORM操作数据-分页查询

### 分页查询概述

分页是处理大量数据的常用技术，通过限制每页返回的记录数来提高性能。

### 基本分页

#### 使用offset和limit

```python
from sqlalchemy.future import select

async def get_items_paged(db: AsyncSession, page: int = 1, page_size: int = 10):
    skip = (page - 1) * page_size
    result = await db.execute(
        select(Item).offset(skip).limit(page_size)
    )
    return result.scalars().all()
```

#### 带总数的分页

```python
async def get_items_with_total(db: AsyncSession, page: int = 1, page_size: int = 10):
    skip = (page - 1) * page_size
    
    # 查询数据
    result = await db.execute(
        select(Item).offset(skip).limit(page_size)
    )
    items = result.scalars().all()
    
    # 查询总数
    count_result = await db.execute(select(func.count(Item.id)))
    total = count_result.scalar()
    
    return {
        "items": items,
        "total": total,
        "page": page,
        "page_size": page_size,
        "total_pages": (total + page_size - 1) // page_size
    }
```

### 使用FastAPI参数验证

```python
from fastapi import Query

@app.get("/items/")
async def get_items(
    page: int = Query(1, ge=1),
    page_size: int = Query(10, ge=1, le=100),
    db: AsyncSession = Depends(get_db)
):
    skip = (page - 1) * page_size
    
    result = await db.execute(
        select(Item).offset(skip).limit(page_size)
    )
    items = result.scalars().all()
    
    count_result = await db.execute(select(func.count(Item.id)))
    total = count_result.scalar()
    
    return {
        "items": items,
        "pagination": {
            "page": page,
            "page_size": page_size,
            "total": total,
            "total_pages": (total + page_size - 1) // page_size
        }
    }
```

**浏览器访问方式：**
- `http://127.0.0.1:8000/items/?page=1&page_size=10` → 返回第1页，每页10条
- `http://127.0.0.1:8000/items/?page=2&page_size=20` → 返回第2页，每页20条

### 优化分页查询

#### 使用keyset分页（游标分页）

```python
async def get_items_cursor(db: AsyncSession, last_id: int = None, page_size: int = 10):
    query = select(Item).order_by(Item.id).limit(page_size)
    
    if last_id:
        query = query.where(Item.id > last_id)
    
    result = await db.execute(query)
    return result.scalars().all()
```

#### 带排序的分页

```python
from sqlalchemy import desc

async def get_items_sorted(
    db: AsyncSession,
    page: int = 1,
    page_size: int = 10,
    sort_by: str = "created_at",
    sort_order: str = "desc"
):
    skip = (page - 1) * page_size
    
    if sort_by == "price":
        order_column = Item.price
    elif sort_by == "name":
        order_column = Item.name
    else:
        order_column = Item.created_at
    
    if sort_order == "asc":
        query = select(Item).order_by(order_column).offset(skip).limit(page_size)
    else:
        query = select(Item).order_by(desc(order_column)).offset(skip).limit(page_size)
    
    result = await db.execute(query)
    return result.scalars().all()
```

### 本章小结

分页查询的实现方式：

1. 使用`offset()`和`limit()`实现基本分页
2. 计算总数并返回分页元数据
3. 使用FastAPI参数验证限制分页参数
4. 使用keyset分页优化大数据量场景

下一章将学习查询总结。

---

## 25. FastAPI进阶 - ORM操作数据-查询总结

### 查询操作总结

#### 查询方法速查表

| 操作 | SQLAlchemy代码 | 说明 |
|------|---------------|------|
| 查询所有 | `select(Item).all()` | 获取所有记录 |
| 查询单条 | `select(Item).where(Item.id == 1).first()` | 获取第一条匹配记录 |
| 条件查询 | `select(Item).where(Item.price > 10)` | 添加WHERE条件 |
| 排序 | `select(Item).order_by(Item.price)` | 按字段排序 |
| 分页 | `select(Item).offset(0).limit(10)` | 跳过前N条，取M条 |
| 聚合 | `select(func.count(Item.id))` | 统计记录数 |
| 分组 | `select(Item.category, func.count(Item.id)).group_by(Item.category)` | 按字段分组 |

#### 条件运算符

| 运算符 | 代码 | 说明 |
|--------|------|------|
| == | `Item.id == 1` | 等于 |
| != | `Item.id != 1` | 不等于 |
| > | `Item.price > 10` | 大于 |
| >= | `Item.price >= 10` | 大于等于 |
| < | `Item.price < 100` | 小于 |
| <= | `Item.price <= 100` | 小于等于 |
| IN | `Item.id.in_([1, 2, 3])` | 在列表中 |
| NOT IN | `~Item.id.in_([1, 2, 3])` | 不在列表中 |
| IS NULL | `Item.description.is_(None)` | 为空 |
| IS NOT NULL | `Item.description.is_not(None)` | 不为空 |

#### 模糊查询

| 方法 | 代码 | 说明 |
|------|------|------|
| LIKE | `Item.name.like("%Foo%")` | 模糊匹配 |
| ILIKE | `Item.name.ilike("%foo%")` | 不区分大小写 |
| CONTAINS | `Item.name.contains("Foo")` | 包含字符串 |
| STARTSWITH | `Item.name.startswith("Foo")` | 以字符串开头 |
| ENDSWITH | `Item.name.endswith("Bar")` | 以字符串结尾 |

#### 逻辑组合

```python
from sqlalchemy import and_, or_, not_

# AND条件
select(Item).where(and_(Item.price > 10, Item.price < 100))

# OR条件
select(Item).where(or_(Item.name == "Foo", Item.name == "Bar"))

# NOT条件
select(Item).where(not_(Item.price < 10))
```

### 查询最佳实践

#### 1. 使用selectinload加载关联数据

```python
from sqlalchemy.orm import selectinload

result = await db.execute(
    select(Item).options(selectinload(Item.category))
)
```

#### 2. 避免N+1查询问题

```python
# 不好的做法 - 会产生N+1次查询
items = await db.execute(select(Item)).scalars().all()
for item in items:
    print(item.category.name)  # 每次访问都会查询

# 好的做法 - 使用预加载
items = await db.execute(
    select(Item).options(selectinload(Item.category))
).scalars().all()
```

#### 3. 使用exists子查询

```python
from sqlalchemy import exists

query = select(Item).where(
    exists().where(Item.id == Order.item_id)
)
```

#### 4. 使用CTE（Common Table Expression）

```python
from sqlalchemy import select

cte = select(Item).where(Item.price > 100).cte("expensive_items")
query = select(cte).where(cte.c.category == "Electronics")
```

### 本章小结

ORM查询的核心要点：

1. 使用`select()`构建查询
2. 使用`where()`添加条件
3. 使用`order_by()`排序
4. 使用`offset()`和`limit()`分页
5. 使用`func`模块进行聚合
6. 使用`group_by()`分组

下一章将学习新增数据的操作。

---

## 26. FastAPI进阶 - ORM操作数据-新增数据

### 新增数据概述

SQLAlchemy提供了多种方式来插入数据到数据库。

### 单条数据插入

#### 基本插入

```python
from sqlalchemy.ext.asyncio import AsyncSession

async def create_item(db: AsyncSession, name: str, price: float):
    item = Item(name=name, price=price)
    db.add(item)
    await db.commit()
    await db.refresh(item)
    return item
```

**浏览器访问方式**（假设将这些函数包装在路由中）：
- 使用curl创建item：`curl -X POST "http://127.0.0.1:8000/items/" -H "Content-Type: application/json" -d '{"name": "New Item", "price": 19.99}'`

#### 使用Pydantic模型

```python
from pydantic import BaseModel

class ItemCreate(BaseModel):
    name: str
    description: str = None
    price: float

async def create_item(db: AsyncSession, item_data: ItemCreate):
    item = Item(**item_data.dict())
    db.add(item)
    await db.commit()
    await db.refresh(item)
    return item
```

### 批量数据插入

#### 使用add_all

```python
async def create_items(db: AsyncSession, items_data: list[dict]):
    items = [Item(**data) for data in items_data]
    db.add_all(items)
    await db.commit()
    return items
```

#### 使用executemany

```python
from sqlalchemy import insert

async def bulk_insert_items(db: AsyncSession, items_data: list[dict]):
    stmt = insert(Item).values(items_data)
    await db.execute(stmt)
    await db.commit()
```

### 插入后获取ID

```python
async def create_item_return_id(db: AsyncSession, name: str, price: float):
    item = Item(name=name, price=price)
    db.add(item)
    await db.flush()  # 刷新但不提交
    print(f"New item ID: {item.id}")
    await db.commit()
    return item
```

### 事务处理

#### 手动事务

```python
async def create_order_with_items(db: AsyncSession, order_data, items_data):
    try:
        # 开始事务
        async with db.begin():
            # 创建订单
            order = Order(**order_data)
            db.add(order)
            await db.flush()
            
            # 创建订单项
            for item_data in items_data:
                order_item = OrderItem(order_id=order.id, **item_data)
                db.add(order_item)
        
        # 事务自动提交
        return order
    except Exception as e:
        # 事务自动回滚
        await db.rollback()
        raise e
```

### 本章小结

新增数据的要点：

1. 使用`add()`添加单条记录
2. 使用`add_all()`批量添加记录
3. 使用`commit()`提交事务
4. 使用`refresh()`获取数据库生成的值（如自增ID）
5. 使用事务确保数据一致性

下一章将学习更新数据的操作。

---

## 27. FastAPI进阶 - ORM操作数据-更新数据

### 更新数据概述

SQLAlchemy提供了多种方式来更新数据库中的数据。

### 更新单条记录

#### 基本更新

```python
async def update_item(db: AsyncSession, item_id: int, name: str, price: float):
    result = await db.execute(select(Item).where(Item.id == item_id))
    item = result.scalar_one_or_none()
    
    if item:
        item.name = name
        item.price = price
        await db.commit()
        await db.refresh(item)
    
    return item
```

**浏览器访问方式**（假设将这些函数包装在路由中）：
- 使用curl更新item：`curl -X PUT "http://127.0.0.1:8000/items/1" -H "Content-Type: application/json" -d '{"name": "Updated Item", "price": 29.99}'`

#### 使用Pydantic模型更新

```python
from pydantic import BaseModel

class ItemUpdate(BaseModel):
    name: str = None
    description: str = None
    price: float = None

async def update_item(db: AsyncSession, item_id: int, update_data: ItemUpdate):
    result = await db.execute(select(Item).where(Item.id == item_id))
    item = result.scalar_one_or_none()
    
    if item:
        # 只更新非None的字段
        update_dict = update_data.dict(exclude_unset=True)
        for key, value in update_dict.items():
            setattr(item, key, value)
        
        await db.commit()
        await db.refresh(item)
    
    return item
```

### 批量更新

#### 使用update语句

```python
from sqlalchemy import update

async def update_items_price(db: AsyncSession, category: str, new_price: float):
    stmt = (
        update(Item)
        .where(Item.category == category)
        .values(price=new_price)
    )
    result = await db.execute(stmt)
    await db.commit()
    return result.rowcount  # 返回更新的行数
```

#### 条件更新

```python
async def discount_items(db: AsyncSession, percentage: float):
    stmt = (
        update(Item)
        .where(Item.price > 100)
        .values(price=Item.price * (1 - percentage))
    )
    result = await db.execute(stmt)
    await db.commit()
    return result.rowcount
```

### 使用merge

#### 合并数据

```python
async def save_item(db: AsyncSession, item_data: dict):
    # 如果item已存在则更新，否则插入
    item = Item(**item_data)
    db_item = await db.merge(item)
    await db.commit()
    return db_item
```

### 本章小结

更新数据的要点：

1. 查询记录后直接修改属性
2. 使用`commit()`提交更改
3. 使用`refresh()`获取最新数据
4. 使用`update()`语句进行批量更新
5. 使用`merge()`进行插入或更新操作

下一章将学习删除数据的操作。

---

## 28. FastAPI进阶 - ORM操作数据-删除数据

### 删除数据概述

SQLAlchemy提供了多种方式来删除数据库中的数据。

### 删除单条记录

#### 基本删除

```python
async def delete_item(db: AsyncSession, item_id: int):
    result = await db.execute(select(Item).where(Item.id == item_id))
    item = result.scalar_one_or_none()
    
    if item:
        await db.delete(item)
        await db.commit()
        return True
    
    return False
```

**浏览器访问方式**（假设将这些函数包装在路由中）：
- 使用curl删除item：`curl -X DELETE "http://127.0.0.1:8000/items/1"`

#### 安全删除

```python
async def safe_delete_item(db: AsyncSession, item_id: int):
    try:
        async with db.begin():
            result = await db.execute(select(Item).where(Item.id == item_id))
            item = result.scalar_one_or_none()
            
            if item:
                await db.delete(item)
                return True
            
            return False
    except Exception as e:
        await db.rollback()
        raise e
```

### 批量删除

#### 使用delete语句

```python
from sqlalchemy import delete

async def delete_items_by_category(db: AsyncSession, category: str):
    stmt = delete(Item).where(Item.category == category)
    result = await db.execute(stmt)
    await db.commit()
    return result.rowcount  # 返回删除的行数
```

#### 条件删除

```python
async def delete_old_items(db: AsyncSession, days: int):
    from datetime import datetime, timedelta
    
    cutoff_date = datetime.utcnow() - timedelta(days=days)
    stmt = delete(Item).where(Item.created_at < cutoff_date)
    result = await db.execute(stmt)
    await db.commit()
    return result.rowcount
```

### 级联删除

#### 配置级联关系

```python
from sqlalchemy import ForeignKey
from sqlalchemy.orm import relationship

class Order(Base):
    __tablename__ = "orders"
    id = Column(Integer, primary_key=True)
    items = relationship("OrderItem", cascade="all, delete-orphan")

class OrderItem(Base):
    __tablename__ = "order_items"
    id = Column(Integer, primary_key=True)
    order_id = Column(Integer, ForeignKey("orders.id"))
```

#### 删除级联数据

```python
async def delete_order_with_items(db: AsyncSession, order_id: int):
    result = await db.execute(select(Order).where(Order.id == order_id))
    order = result.scalar_one_or_none()
    
    if order:
        await db.delete(order)  # 会自动删除关联的OrderItem
        await db.commit()
        return True
    
    return False
```

### 本章小结

删除数据的要点：

1. 查询记录后使用`delete()`删除
2. 使用`commit()`提交更改
3. 使用`delete()`语句进行批量删除
4. 配置级联关系实现级联删除
5. 使用事务确保数据一致性

下一章将学习ORM总结。

---

## 29. FastAPI进阶 - ORM总结

### ORM操作总结

#### CRUD操作速查表

| 操作 | SQL | SQLAlchemy代码 |
|------|-----|---------------|
| 创建 | INSERT INTO | `db.add(item)` |
| 读取 | SELECT | `select(Item).where(...)` |
| 更新 | UPDATE | `item.attr = value` |
| 删除 | DELETE | `db.delete(item)` |

#### 数据库会话操作

```python
# 创建会话
async def get_db():
    async with async_session() as session:
        yield session

# 查询
result = await db.execute(select(Item))
items = result.scalars().all()

# 插入
item = Item(name="Foo", price=35.4)
db.add(item)
await db.commit()
await db.refresh(item)

# 更新
item.name = "Bar"
await db.commit()

# 删除
await db.delete(item)
await db.commit()
```

#### 查询进阶操作

| 操作 | 代码示例 |
|------|---------|
| 条件查询 | `select(Item).where(Item.price > 10)` |
| 排序 | `select(Item).order_by(Item.price)` |
| 分页 | `select(Item).offset(0).limit(10)` |
| 聚合 | `select(func.count(Item.id))` |
| 分组 | `select(Item.category, func.count(Item.id)).group_by(Item.category)` |

### ORM最佳实践

#### 1. 使用异步模式

```python
# 使用异步引擎
engine = create_async_engine(DATABASE_URL)

# 使用异步会话
async_session = sessionmaker(engine, class_=AsyncSession)
```

#### 2. 使用Pydantic模型

```python
from pydantic import BaseModel

class ItemCreate(BaseModel):
    name: str
    price: float

class ItemResponse(BaseModel):
    id: int
    name: str
    price: float
    
    class Config:
        orm_mode = True
```

#### 3. 使用依赖注入

```python
from fastapi import Depends

async def get_db():
    async with async_session() as session:
        yield session

@app.get("/items/")
async def read_items(db: AsyncSession = Depends(get_db)):
    pass
```

#### 4. 错误处理

```python
from fastapi import HTTPException

async def get_item(db: AsyncSession, item_id: int):
    result = await db.execute(select(Item).where(Item.id == item_id))
    item = result.scalar_one_or_none()
    
    if not item:
        raise HTTPException(status_code=404, detail="Item not found")
    
    return item
```

#### 5. 事务管理

```python
async def create_order(db: AsyncSession, order_data):
    try:
        async with db.begin():
            order = Order(**order_data)
            db.add(order)
            await db.flush()
            # 其他操作
        return order
    except Exception as e:
        await db.rollback()
        raise e
```

### ORM总结

通过本章节的学习，我们掌握了：

1. **ORM基础**：SQLAlchemy的基本概念和配置
2. **模型定义**：如何定义数据库表和字段
3. **CRUD操作**：创建、读取、更新、删除数据
4. **查询进阶**：条件查询、聚合、分组、分页
5. **事务管理**：确保数据一致性
6. **最佳实践**：异步模式、依赖注入、错误处理

ORM是现代Web开发的重要工具，它简化了数据库操作，提高了开发效率，同时保持了代码的可维护性和可扩展性。

---

## 30. FastAPI进阶 - 对接LangChain Chroma向量数据库

### LangChain Chroma简介

LangChain提供了对Chroma向量数据库的集成支持，通过`langchain-chroma`包可以方便地在LangChain生态中使用Chroma。它特别适合以下场景：

- 语义搜索
- RAG（检索增强生成）
- 推荐系统
- 相似度匹配

### 安装依赖

```bash
pip install langchain-chroma fastapi uvicorn
```

### 初始化LangChain Chroma

```python
from langchain_chroma import Chroma
from langchain_core.documents import Document

db = Chroma(
    collection_name="documents",
    persist_directory="./chroma_data"
)
```

**浏览器访问方式**：这是后端初始化代码，启动后可通过API访问。

### 创建集合并添加文档

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional, Dict

app = FastAPI()

class AddDocumentsRequest(BaseModel):
    documents: List[str]
    metadatas: Optional[List[Dict]] = None
    ids: Optional[List[str]] = None

@app.post("/add-documents/")
async def add_documents(request: AddDocumentsRequest):
    try:
        docs = [
            Document(page_content=doc, metadata=meta if request.metadatas else {})
            for i, doc in enumerate(request.documents)
        ]
        
        db = Chroma(
            collection_name="documents",
            persist_directory="./chroma_data"
        )
        
        db.add_documents(
            documents=docs,
            ids=request.ids
        )
        
        return {"message": f"成功添加 {len(request.documents)} 条文档"}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))
```

**浏览器访问方式**：
- 使用curl添加文档：`curl -X POST "http://127.0.0.1:8000/add-documents/" -H "Content-Type: application/json" -d '{"documents": ["这是第一篇文档", "这是第二篇文档"], "metadatas": [{"source": "doc1"}, {"source": "doc2"}]}'`

### 查询相似文档

```python
class QueryRequest(BaseModel):
    query: str
    n_results: int = 2

@app.post("/query/")
async def query_documents(request: QueryRequest):
    try:
        db = Chroma(
            collection_name="documents",
            persist_directory="./chroma_data"
        )
        
        results = db.similarity_search(
            query=request.query,
            k=request.n_results
        )
        
        formatted_results = [
            {
                "content": doc.page_content,
                "metadata": doc.metadata
            }
            for doc in results
        ]
        
        return {"results": formatted_results}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))
```

**浏览器访问方式**：
- 使用curl查询文档：`curl -X POST "http://127.0.0.1:8000/query/" -H "Content-Type: application/json" -d '{"query": "查询关键词"}'`

### 删除文档

```python
class DeleteDocumentsRequest(BaseModel):
    ids: List[str]

@app.delete("/delete-documents/")
async def delete_documents(request: DeleteDocumentsRequest):
    try:
        db = Chroma(
            collection_name="documents",
            persist_directory="./chroma_data"
        )
        
        db.delete(ids=request.ids)
        return {"message": f"成功删除 {len(request.ids)} 条文档"}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))
```

**浏览器访问方式**：
- 使用curl删除文档：`curl -X DELETE "http://127.0.0.1:8000/delete-documents/" -H "Content-Type: application/json" -d '{"ids": ["id1", "id2"]}'`

### 获取集合信息

```python
@app.get("/collection-count/")
async def get_collection_count():
    try:
        db = Chroma(
            collection_name="documents",
            persist_directory="./chroma_data"
        )
        
        count = db._collection.count()
        return {"collection": "documents", "count": count}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))
```

**浏览器访问方式**：
- `http://127.0.0.1:8000/collection-count/` → 返回集合中文档数量

### 完整可运行文件（chroma_app.py）

将以下代码保存为 `chroma_app.py`，可以直接运行：

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional, Dict
from langchain_chroma import Chroma
from langchain_core.documents import Document

app = FastAPI(title="LangChain Chroma向量数据库API")

class AddDocumentsRequest(BaseModel):
    documents: List[str]
    metadatas: Optional[List[Dict]] = None
    ids: Optional[List[str]] = None

class QueryRequest(BaseModel):
    query: str
    n_results: int = 2

class DeleteDocumentsRequest(BaseModel):
    ids: List[str]

def get_chroma_db(collection_name: str = "documents"):
    return Chroma(
        collection_name=collection_name,
        persist_directory="./chroma_data"
    )

@app.post("/add-documents/", response_model=dict)
async def add_documents(request: AddDocumentsRequest):
    try:
        docs = [
            Document(page_content=doc, metadata=request.metadatas[i] if request.metadatas else {})
            for i, doc in enumerate(request.documents)
        ]
        
        db = get_chroma_db()
        db.add_documents(documents=docs, ids=request.ids)
        
        return {"message": f"成功添加 {len(request.documents)} 条文档"}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))

@app.post("/query/", response_model=dict)
async def query_documents(request: QueryRequest):
    try:
        db = get_chroma_db()
        results = db.similarity_search(query=request.query, k=request.n_results)
        
        formatted_results = [
            {
                "content": doc.page_content,
                "metadata": doc.metadata
            }
            for doc in results
        ]
        
        return {"results": formatted_results}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))

@app.delete("/delete-documents/", response_model=dict)
async def delete_documents(request: DeleteDocumentsRequest):
    try:
        db = get_chroma_db()
        db.delete(ids=request.ids)
        return {"message": f"成功删除 {len(request.ids)} 条文档"}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))

@app.get("/collection-count/", response_model=dict)
async def get_collection_count():
    try:
        db = get_chroma_db()
        count = db._collection.count()
        return {"collection": "documents", "count": count}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))

@app.delete("/clear-collection/", response_model=dict)
async def clear_collection():
    try:
        db = get_chroma_db()
        db.delete_collection()
        return {"message": "集合已清空"}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

**启动命令：**
```bash
uvicorn chroma_app:app --reload
```

**浏览器访问方式汇总：**

| 操作 | 方法 | URL | 示例 |
|------|------|-----|------|
| 添加文档 | POST | `/add-documents/` | `curl -X POST "http://127.0.0.1:8000/add-documents/" -H "Content-Type: application/json" -d '{"documents": ["这是测试文档"]}'` |
| 查询文档 | POST | `/query/` | `curl -X POST "http://127.0.0.1:8000/query/" -H "Content-Type: application/json" -d '{"query": "搜索关键词"}'` |
| 删除文档 | DELETE | `/delete-documents/` | `curl -X DELETE "http://127.0.0.1:8000/delete-documents/" -H "Content-Type: application/json" -d '{"ids": ["id1", "id2"]}'` |
| 获取数量 | GET | `/collection-count/` | `http://127.0.0.1:8000/collection-count/` |
| 清空集合 | DELETE | `/clear-collection/` | `curl -X DELETE "http://127.0.0.1:8000/clear-collection/"` |

### LangChain Chroma最佳实践

#### 1. 使用持久化存储

```python
from langchain_chroma import Chroma

db = Chroma(
    collection_name="documents",
    persist_directory="./chroma_data"
)
```

#### 2. 使用自定义嵌入函数

```python
from langchain_chroma import Chroma
from langchain_openai import OpenAIEmbeddings

embeddings = OpenAIEmbeddings(model="text-embedding-3-small")

db = Chroma(
    collection_name="documents",
    persist_directory="./chroma_data",
    embedding_function=embeddings
)
```

#### 3. 使用过滤条件查询

```python
from langchain_chroma import Chroma

db = Chroma(
    collection_name="documents",
    persist_directory="./chroma_data"
)

results = db.similarity_search(
    query="查询关键词",
    k=2,
    filter={"source": "doc1.txt"}
)
```

#### 4. 处理大量数据

```python
from langchain_chroma import Chroma
from langchain_core.documents import Document

db = Chroma(
    collection_name="documents",
    persist_directory="./chroma_data"
)

batch_size = 1000
for i in range(0, len(documents), batch_size):
    batch_docs = [
        Document(page_content=doc)
        for doc in documents[i:i+batch_size]
    ]
    batch_ids = [f"id_{j}" for j in range(i, i+batch_size)]
    db.add_documents(documents=batch_docs, ids=batch_ids)
```

#### 5. 使用RAG检索链

```python
from langchain.chains import RetrievalQA
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-4o-mini")
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type="stuff",
    retriever=db.as_retriever()
)

response = qa_chain.run("用户问题")
```

### LangChain Chroma总结

通过本章节的学习，我们掌握了：

1. **LangChain Chroma基础**：向量数据库的概念和用途
2. **初始化配置**：使用`langchain_chroma.Chroma`进行持久化存储
3. **文档操作**：添加、查询、删除文档
4. **高级功能**：自定义嵌入函数、过滤查询、批量操作
5. **RAG集成**：与LangChain检索链结合实现问答功能

LangChain Chroma是构建AI应用的强大工具，它与FastAPI的结合可以快速实现语义搜索、RAG等功能。

---

恭喜您完成了FastAPI从入门到实战的全部课程！