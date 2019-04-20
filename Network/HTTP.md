## 一、HTTP简介
### 1.1 定义
   > HTTP是一种无状态的、由文本构成的请求-响应（request-response）协议，这种协议使用的是客户端-服务器（client-server）计算模型
   
* 解读
    1. 无状态的：后序发送的请求对之前发送过的请求一无所知（session --> cookie保存状态）
    2. 文本：http是以纯文本方式而非二进制方式传送数据的，为了让开发者无需借助专门的协议分析工具便可获知通信中发生的事情（传输的数据），便于排查故障
    3. 请求-响应：通信基本方式
    4. 客户端：用户代理、web浏览器、请求方
    5. 服务器：响应方
### 1.2 示图
   ![](https://github.com/Duk1906/Learning_Records/blob/master/Pictures/http.png)

## 二、HTTP请求
### 2.1 请求方法
   1. GET：获取
   2. POST：提交或修改
   3. CONNECT：命令服务器与客户端建立网络连接，常用与设置SSL隧道以开启HTTPS功能(记录一下，没用过)
   4. PUT、DELETE....
   5. GET vs POST
      1. 两者都可以处理表单，但由于GET请求不包含主体，GET方法传递的表单数据以键值对形式存放在请求URL里
   6. 其他点
       1. Request Method: GET
       2. 安全的请求方法：只取不改为安全，GET是，POST不是
       3. 幂等的请求方法：使用相同数据，再次调用方法，服务器状态不变，如PUT和DELETE
### 2.2 请求首部
    Accept:  接受内容类型
    Accept-Encoding: 客户端要求服务器使用的编码
    Cookie: 客户端应把服务器所有设置的cookie回传给服务器
    Host: 服务名+端口号
    Referer: 发起请求页面所在地址
    User-Agent: 客户端信息
    X-Requested-With: XMLHttpRequest(支持XML及JSON各种格式的请求与响应)

## 三、HTTP响应
### 3.1状态码
    1xx：情报状态码(旨在告诉客户端已收到请求并已处理)
    2xx：成功状态码(200)
    3xx：重定向状态码(URL重定向)
    4xx：客户端错误状态码(404找不到资源)
    5xx：服务器错误状态码(500代码出错会报该错误)
### 3.2响应首部
    Allow：支持的请求方法
    Content-Length：响应主体字节长度
	Content-Type: text/html; charset=utf-8
	Date: Thu, 24 Jan 2019 14:19:46 GMT
	Server: GitHub.com
	Set-Cookie: 与请求首部Cookie对应
## 四、URI
> URI(统一资源标识符) = URN(统一资源名称) + URL(统一资源定位符)

> 格式：方案：//用户信息@分层部分?查询参数#片段

> 示例：http://lixiangpeng:66666@www.baidu.com/pic/http?name=xp&sex=0#summary

> 注：只有方案和分层是必须的

> 片段：对URI定义的资源中的次级资源进行标识
   
