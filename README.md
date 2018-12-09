# Nginx
---
## 主要内容
- 是什么
- 能做什么
## 一.是什么
>Nginx(engine x)是一款轻量级的 ***Web服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器***，并在一个BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上nginx的并发能力确实在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有： ***百度、京东、新浪、网易、腾讯、淘宝 ***等
## 二.能做什么
* 反向代理
* 负载均衡
* HTTP服务器（包含动静分离）
* 正向代理
### 1.反向代理
   * 定义：指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。
   * 简单来说就是真实的服务器不能直接被外部网络访问，所以需要一台代理服务器，而代理服务器能被外部网络访问的同时又跟真实服务器在同一个网络环境，当然也可能是同一台服务器，端口不同而已
   * 图解： ![](http://static.open-open.com/lib/uploadImg/20141202/20141202104859_484.jpg)
   * 作用
      - 保护网站安全
      - 通过配置缓存功能加速Web请求
      - 实现负载均衡：![](http://static.open-open.com/lib/uploadImg/20141202/20141202104900_44.jpg)
      
### 2.负载均衡
   * 定义：将请求分摊到多个操作单元上进行执行，例如Web服务器、FTP服务器、企业关键应用服务器和其它关键任务服务器等，从而共同完成工作任务
   * Nginx自带3种负载均衡策略
     - RR（默认）：每个请求按时间顺序逐一分配到不同的后端服务器
     - 权重：指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况
     - ip_hash：确保每个访客固定访问一个后端服务器，可以解决session的问题
     
### 3.HTTP服务器
   * Nginx本身也是一个静态资源的服务器
   * 动静分离：让动态网站里的动态网页根据一定规则把不变的资源和经常变的资源区分开来，动静资源做好了拆分以后，我们就可以根据静态资源的特点将其做缓存操作，这就是网站静态化处理的核心思路
   
### 4.正向代理
   * 一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端才能使用正向代理
   * ***感觉和反向代理异曲同工***

## 参考资源
  * 算法爱好者（公众号）
  * http://www.open-open.com/lib/view/open1417488526633.html
