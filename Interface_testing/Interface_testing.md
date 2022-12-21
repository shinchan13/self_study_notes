# 接口

- **概念：接口就是数据交互的通道**。在系统或组件之间，完成数据的传递。

- 接口的类型：1. 按协议划分。协议不同接口类型不同，HTTP, TCP, UDP(无连接的传输协议), IP, FTP, USB等 

  ​				       2. 按语言换划分。java，python等

  ​					   3. 按范围划分。系统之间或者程序内部之间

- 接口测试的概念：测试系统或组件之间交互的 数据的 正确性，以及逻辑依赖关系的正确性。

- 接口测试的原理：使用 工具或代码 模拟客户端，组织数据，发送请求给服务器，校验服务器回发的响应数据是否与预期一致。

- 接口风格：

  - 传统：只用get和post，URL不唯一
  - RESTful：使用get，post，put，delete，URL唯一

# HTTP请求

## **作用**

- 由客户端 发送 给服务器
- 规定了客户端 发送给 服务器的 语法格式

## **整体格式**

组成部分：请求行，请求头，空行，请求体

![1](.\data\1.jpg)

![](.\data\2.jpg)



## 请求行

作用：指定请求方法，请求资源

请求方法：

- get 查询（没有请求体）
- post 新增（登录注册使用，有请求体）
- put 修改（有请求体）
- delete 删除（没有请求体）

协议版本：http1.1，http1.2，http2.0（主要使用http1.1）



## 请求头

- 作用：向 服务器 描述 客户端（浏览器） 的基本信息
- User-Agent：向 服务器 描述 浏览器 的类型
- Content-Type：向 服务器 描述 请求体 的数据类型



## 请求体

- get，delete 请求方法 没有 请求体
- post，put 请求方法 有 请求体
- 请求体的 数据类型 受 请求头中 Content-Type的值影响



# HTTP响应

## 作用

- 由 服务器 回发送给 客户端
- 规定了 服务器 回发送给 客户端的 数据的 语法格式

## 整体格式

组成部分：响应行（状态行），响应头，空行，响应体

![](.\data\3.jpg)



![](.\data\4.jpg)

## 响应行（状态行）

协议版本：http1.0, http1.1, http2.0，常用http1.1

状态码：针对 http请求 所 响应的状态

- 1xx：信息类，请求需要进一步访问
- 2xx：成功，200表示ok
- 3xx：重定向，数据资源需要重新定向访问
- 4xx：客户端错误，404 文件或资源不存在，403 文件或者资源存在，但是没有访问的权限
- 5xx：服务器错误

状态码描述：对状态码的 说明



## 响应头

- 作用：向 客户端 描述 服务器的 基本信息
- Content-Type：向 客户端 描述 响应体的 数据类型



## 响应体

- http响应大多数是有 响应体的
- 响应体的 数据类型 受 响应头中的 Content-Type 的值影响
- 常见的类型：
  - json
  - 表单
  - 图片







# 接口测试流程

1. 结合需求文档进行**需求分析**
2. 结合开发人员编写的**接口文档** 进行 接口分析
3. 结合 **需求分析和接口文档** 设计接口测试用例
4. 执行测试 (使用接口测试工具实现 或者 通过代码实现)
5. 接口缺陷管理与跟踪
6. 生成测试报告
7. 接口自动化持续集成



# 接口文档内容

- 作用
  - 请后端协作
  - 人员更迭
  - 方便编写测试用例

- 基本信息

  > 请求方法、URL（协议+域名+资源路径）、接口描述

- 请求参数

  > 请求头，请求体（get和delete没有请求体）

- 返回数据

  > http响应回来的状态码和状态描述
  >
  > 响应体



# 接口文档解析

**分析接口文档，找出http请求 和http 响应 所需要的数据**

**http请求：**

- 请求行
  - 请求方法
  - URL
  - 协议版本：默认http1.1

- 请求头
  - Content-Type（指定 请求体的 数据类型）
- 请求体

**http应答：**

- 响应行
  - 状态码，状态描述
- 响应头（测试过程中不重要）
- 响应体



# 接口用例设计

## 为什么要写

- 防止漏测
- 管理工作进度，评估工作量



## 接口测试的测试点（测试维度）

![](.\data\5.jpg)









# Postman

安装好postman之后 安装nodejs+newman（生成测试报告）

> x-www-form-urlencoded 表单格式
>
> authorization （单词意义 批准）令牌



## postman断言

- 断言：让程序判断 预期结果 和 实际结果 是否一致
- postman断言：利用postman自带的断言机制，判断 预期结果 和 实际结果 是否一致
- postman的断言是使用 Java script 语言编写的，写在“tests”里



## postman常用的断言

1. 断言响应 状态码 (status code is 200)

```javascript
//pm:postman的一个实例
//test():postman实例的方法，有两个参数
//参数一："Status code is 200"断言完成后，给出的提示信息
//参数二：function () {pm.response.to.have.status(200)} 匿名函数的调用
//pm.response.to.have.status(200) postman的响应结果中有 状态码200

pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

  

2. 断言**响应体** 是否包含 某个**字符串**（response body: contains string）

```javascript
//pm.expect(pm.response.text()).to.include("string_you_want_to_search") postman实例预期结果中包含"string_you_want_to_search"(预期结果)字符串

pm.test("Body matches string", function () {
    pm.expect(pm.response.text()).to.include("string_you_want_to_search");
});
```



3. 断言**响应体** 是否等于 某个**字符串或对象**（response body: is equal to a string）

```javascript
pm.test("Body is correct", function () {
    pm.response.to.have.body("response_body_string");
});
```



4. 断言**响应体**中的 Json 数据 (response body: json value check)

```javascript
//var jsonData = pm.response.json();定义一个jsondata变量, 值为json格式的 响应体数据
// pm.expect(jsonData.value).to.eql(100); value 和 100 是变化的，对应响应体中的key:value

pm.test("Your test name", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.value).to.eql(100);
});
```



5. 断言**响应头**(response headers: Content-Type header check )

```js
pm.test("Content-Type is present", function () {
    pm.response.to.have.header("Content-Type");
});
```





## postman断言工作原理

1. 点击`send`按钮时候，http请求（行，头，体）和断言代码进入postman内部
2. postman内部 解析http请求，然后 发送http请求 到服务器
3. 服务器接收并解析http请求，然后回发响应结果
4. postman内部收到并解析 http响应，然后断言
5. 返回到postman前端界面，显示http响应（行，头，体）和断言结果

![](.\data\6.jpg)

## 全局变量和环境变量

- 全局变量：全局唯一，不可重复
- 环境变量：
  - 一个变量只能属于某个环境，在某一个环境中 不可重复
  - 环境与环境之间可以定义重复的变量
  - 一个环境可以包含多个环境变量
  - 常见的环境分类：开发环境，测试环境，生产环境



> 设置全局变量：pm.globals.set("var_name", value)  var_name全局变量名，需要加上“ ”
>
> 设置环境变量：pm.environment.set("var_name", value)  var_name环境变量名，需要加上“ ”



**获取变量值**

- 全局变量
  - 请求参数中获取：{{var_name}}
  - 代码中获取：var value = pm.globals.get("var_name")
- 环境变量
  - 请求参数中获取：{{var_name}}
  - 代码中获取：var_value = pm.environment.get("var_name")





# postman请求前置脚本

> 假设一种场景：
>
> ​		调用某接口的时候，要输入”时间戳“，如果输入的”时间戳“的绝对值，超过标准时间十分钟，则不允许调用



## 概念

- 时间戳：表示当前系统时间。表示方式：从1970年1月1日00：00：00 ~ 现在  所经历的秒数 （unix操作系统诞生的时间）
- 请求前置脚本：
  - 书写在”pre-request-script"标签中
  - 在http请求发送之前，会自动执行”pre-request-script"中的代码



## 请求前置脚本

```js
//获取时间戳

var timestemp = new Date().getTime()

//将时间保存到 全局变量中

pm.globals.set("gbl_timestemp", timestemp)
```

> 可在 控制 窗口查看



实现步骤：

1. 创建http请求页
2. 指定请求方法，url
3. 在pre-request-script标签中写入代码
4. 点击send按钮，自动执行pre-request-script标签中的代码
5. 查看变量
6. 在url中使用全局变量{{}}
7. console





# postman关联

postman中的关联 是用来解决 接口和接口之间调用 依赖关系



## 实现步骤

以A接口 返回的数据，供B接口使用：

1. 组织A接口http请求数据，发送A接口http请求
2. 获取A接口 返回的 响应数据，写入全局变量或者环境变量（tests）
3. 组织B接口http请求数据，从全局变量或者环境变量中 获取A返回的数据





# 批量执行测试用例

只需要点击Run



# postman测试报告



```shell
newman run 测试用例集名.json    -e 环境变量文件  -d 数据文件 -r html  --reporter-html-export  测试报告名称.html
-e 环境变量文件  -d 数据文件 如果没有可省略
测试报告名称.html  自行指定名称
如果添加-r html 报错，是因为没有安装 newman-reporter-html插件
```



生成报告步骤：

1. 导出用例集
2. 在用例集所在目录 地址栏输入cmd 打开终端
3. 键入命令，生成报告







# curl

clicent 的url，用来请求 服务器

```shell
curl --help 
curl      # 显示网页的源代码
curl -i   # 显示响应的行头体
cutl -I   # 显示响应的行头
```

