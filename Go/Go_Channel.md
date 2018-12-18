## Go通道_channel
### 一.创建
##### 1.1 结构
* dataqsiz -- 缓冲槽大小（可存取数据项数量）
* buf -- 缓冲槽指针
* elemsize -- 数据项大小
* elemtype -- 数据项类型
* qcount -- 缓冲槽有效数据项数量
* closed -- 是否关闭
* sendx -- 缓冲槽发送位置索引
* recvx -- 缓冲槽接收位置索引
* sendq -- 发送者等待队列
* recvq -- 接收者等待队列
##### 1.2 创建
    初始化结构信息并分配内存
##### 1.3 类型
    ChannelType = ( "chan" | "chan" "<-" | "<-" "chan" ) ElementType
##### 1.4 设置容量
    c1 := make(chan string, 10)
### 二.收发
##### 2.1同步
* 关键是找到匹配的接收方或发送方，找到直接copy数据
* 没找到就将自身打包后放入等待队列，由另一方复制并唤醒
* 唤醒后需要验证唤醒者身份，以此决定是否有下一步的实际数据的传输
##### 2.2异步
* 异步模式围绕缓冲槽进行
* 有空位，发送者向槽中复制数据
* 有数据后，接收者从槽中获取数据
* 双方都有唤醒排队中的另一方继续工作的义务
##### 2.3区别
     是否有缓冲槽
##### 2.4关闭通道
* 设置关闭标志(不可重复关闭)
* 释放所有接收者及发送者
* 从这个关闭的channel中不但可以读取出已发送的数据，还可以不断的读取零值

eg：

    c := make(chan int, 10)
    c <- 1
    c <- 2
    close(c)
    fmt.Println(<-c) //1
    fmt.Println(<-c) //2
    fmt.Println(<-c) //0
    fmt.Println(<-c) //0
* 但是如果通过range读取，channel关闭后for循环会跳出
* 通过i, ok := <-c可以查看Channel的状态，判断值是零值还是正常读取的值

### 三.选择
##### 3.1定义
> select语句选择一组可能的send操作和receive操作去处理。（来自菜鸟教程）

* 如果有同时多个case去处理，Go会伪随机的选择一个case处理(pseudo-random)。
* 如果没有case需要处理，则会选择default去处理，如果default case存在的情况下。
* 如果没有default case，则select语句会阻塞，直到某个case需要处理
* select语句只会选择一个case来处理，如果想一直处理channel，你可以在外面加一个无限的for循环

eg：

    for {
        select {
        case c <- x:
             x, y = y, x+y
        case <-quit:
             fmt.Println("quit")
             return
        }
    }

* timeout 或 Ticker

eg：  

    import "time"
    import "fmt"
    func main() {
         c1 := make(chan string, 1)
         go func() {
            time.Sleep(time.Second * 2)
            c1 <- "result 1"
         }()
         select {
         case res := <-c1:
              fmt.Println(res)
         case <-time.After(time.Second * 1):
              fmt.Println("timeout 1")
         }  
    }
