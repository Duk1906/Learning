#### 网络五层协议
![](https://github.com/Duk1906/Learning_Records/blob/master/Pictures/network.png)
#### tcp通信
![](https://github.com/Duk1906/Learning_Records/blob/master/Pictures/tcp1.png)
#### 三次握手与四次握手
    seq和ack号存在于TCP报文段的首部中，seq是序号，是为了连接以后传送数据用的；ack是确认号，值是等待接收的数据包的序列号；大小均为4字节。
    seq：占 4 字节，序号范围[0，2^32-1]，序号增加到 2^32-1 后，下个序号又回到 0。
         TCP 是面向字节流的，通过 TCP 传送的字节流中的每个字节都按顺序编号，而报头中的序号字段值则指的是本报文段数据的第一个字节的序号。
    ack：占 4 字节，期望收到对方下个报文段的第一个数据字节的序号
![](https://github.com/Duk1906/Learning_Records/blob/master/Pictures/tcp2.png)

   TIME_WAIT状态：2MSL等待状态，MSL：maximum segment lifetime(最大分节生命期）
#### udp通信
   *    UDP（user datagram protocol）的中文叫用户数据报协议，属于传输层。UDP是面向非连接的协议，它不与对方建立连接，而是直接把我要发的数据报发给对方。所以UDP适用于一次传输数据量很少、对可靠性要求不高的或对实时性要求高的应用场景。正因为UDP无需建立类如三次握手的连接，而使得通信效率很高。
   *    UDP的应用非常广泛，比如一些知名的应用层协议（SNMP、DNS）都是基于UDP的，想一想，如果SNMP使用的是TCP的话，每次查询请求都得进行三次握手，这个花费的  时间估计是使用者不能忍受的，因为这会产生明显的卡顿。所以UDP就是SNMP的一个很好的选择了，要是查询过程发生丢包错包也没关系的，我们再发起一个查询就好了，因为丢包的情况不多，这样总比每次查询都卡顿一下更容易让人接受吧
![](https://images2015.cnblogs.com/blog/1093303/201701/1093303-20170115195937619-2089905370.jpg)

#### tcp vs udp
* tcp的connect利用三次握手建立连接，更可靠的传输数据。比如邮件等
* udp不属于连接型协议，因而具有资源消耗小，处理速度快的优点，所以通常音频、视频和普通数据在传送时使用UDP较多，比如QQ语音、直播
