## 并发调度
>  ***因为Goroutine，Go才与众不同***。  
> ***Go的一切都在以并发形式运行，垃圾回收、系统监控、网络通信等等***
## 基本图示
                             +-------------sysmon/监控线程--------------+
                             |                                         |
                             |                                         |
    go func() ----> G -----> P|local <---balance---> P|global <---//---P|M
                   并发任务   本地队列                 全局队列            绑定
                             |                        |                 |
                             |                        |                 |
                             +--> M <--------findrunable----steal <--//-+
                                  系统线程     惯偷/从其他P的global部分加锁偷盗
                                  |
                                  |
             +--- execute <--- schedule
             |                  调度器
             |                    |
             +-> G.fn -> goexit --+
                                   