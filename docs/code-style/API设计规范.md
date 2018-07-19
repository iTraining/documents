# 简介

`rest`是一种软件架构风格，如果你们的接口是`rest`接口，那么就可被认为你们的的接口是restful的，英文名词和形容词的区别。

`rest`接口是围绕“资源”展开的，利用HTTP的协议，其实rest本也可以和HTTP无关，但是现在大家普遍的使用`rest`都是依托于HTTP协议。HTTP 的url即资源。

`RFC 3986`定义了通用的URI语法：

```
URI = scheme “://” authority “/” path [ “?” query ][ “#” fragment ]
```

- scheme: 指底层用的协议，如http、https、ftp
- host: 服务器的IP地址或者域名
- port: 端口，http中默认80
- path: 访问资源的路径，就是咱们各种web 框架中定义的route路由
- query: 为发送给服务器的参数
- fragment: 锚点，定位到页面的资源，锚点为资源id

# RESTful API设计

### 资源路径

对于rest资源的定义，即URL的定义，是最重要的；想要设计出优雅的、易读的rest 接口，其实还是听不容易的。

### URL中不能有动词

在Restful架构中，每个网址代表的是一种资源，所以网址中不能有动词，只能有名词，动词由HTTP的 get、post、put、delete 四种方法来表示。

### URL结尾不应该包含斜杠“/”

这是作为URL路径中处理中最重要的规则之一，正斜杠（/）不会增加语义值，且可能导致混淆。REST API不允许一个尾部的斜杠，不应该将它们包含在提供给客户端的链接的结尾处。
许多Web组件和框架将平等对待以下两个URI：
<http://api.canvas.com/shapes/>
<http://api.canvas.com/shapes>

但是，实际上URI中的每个字符都会计入资源的唯一身份的识别中。

两个不同的URI映射到两个不同的资源。如果URI不同，那么资源也是如此，反之亦然。因此，REST API必须生成和传递精确的URI，不能容忍任何的客户端尝试不精确的资源定位。

有些API碰到这种情况，可能设计为让客户端重定向到相应没有尾斜杠的URI（也有可能会返回301 - 用来资源重定向）。

### 正斜杠分隔符”/“必须用来指示层级关系

rul的路径中的正斜杠“/“字符用于指示资源之间的层次关系。
例如：
<http://api.user.com/schools/grades/classes/boys> - 学校中所有的男生

<http://api.college.com/students/3248234/courses> - 检索id为3248234的学生学习的所有课程的清单。

### 应该使用连字符”-“来提高URL的可读性，而不是使用下划线”_”

为了使URL容易让人们理解，请使用连字符”-“字符来提高长路径中名称的可读性。
一些文本查看器为了区分强调URI，常常会在URI下加上下划线。这样下划线”_”字符可能被文本查看器中默认的下划线部分地遮蔽或完全隐藏。
为避免这种混淆，请使用连字符”-“而不是下划线

### URL路径中首选小写字母

RFC 3986将URI定义为区分大小写，但scheme 和 host components除外。

### URL路径名词均为复数

为了保证url格式的一致性，建议使用复数形式。

# RESTful API对资源的操作

对于rest api资源的操作，由HTTP动词表示

### CURD操作

- GET: 获取资源
- POST： 新建资源
- PUT：在服务器更新资源（向客户端提供改变后的所有资源）
- PATCH: 在服务器更新资源（向客户端提供改变的属性）
- DELETE：删除资源

`PATCH`一般不用，用`PUT`

### 资源过滤

在获取资源的时候，有可能需要获取某些“过滤”后的资源，例如指定前10行数据

<http://api.user.com/schools/grades/classes/boys?page=1&page-size=10>

### 返回状态码推荐标准HTTP状态码

有很多服务器将返回状态码一直设为200，然后在返回body里面自定义一些状态码来表示服务器返回结果的状态码。由于rest api是直接使用的HTTP协议，所以它的状态码也要尽量使用HTTP协议的状态码。

- 200 OK 服务器返回用户请求的数据，该操作是幂等的
- 201 CREATED 新建或者修改数据成功
- 204 NOT CONTENT 删除数据成功
- 400 BAD REQUEST 用户发出的请求有问题，该操作是幂等的
- 401 Unauthoried 表示用户没有认证，无法进行操作
- 403 Forbidden 用户访问是被禁止的
- 422 Unprocesable Entity 当创建一个对象时，发生一个验证错误
- 500 INTERNAL SERVER ERROR 服务器内部错误，用户将无法判断发出的请求是否成功
- 503 Service Unavailable 服务不可用状态，多半是因为服务器问题，例如CPU占用率大，等等

### 返回结果

- GET /collections 返回资源列表
- GET /collections/:id 返回单独的资源
- POST /collections 返回新生成的资源对象
- PUT /collections/:id 返回完整的资源对象
- PATCH /collections/:id 返回被修改的属性
- DELETE /collections/:id 返回一个空文档