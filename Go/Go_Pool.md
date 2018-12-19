## 缓存池
* 除避免内存分配操作开销外，更多是为了避免分配大量临时对象时对垃圾回收器造成的影响
* 是一个深入runtime内核运作机制的sync.Pool
### 一. 初始化
##### 1.1 poolLocal
* 定义：用于提供本地缓存对象分配，总是和P/*Pool绑定，为当前工作线程快速无锁分配。
* 结构：

  poolLocal

    type poolLocal struct {
         private   interface{}     //私有缓存区
         shared    []interface{}   //可共享缓存区
         Mutex                     //锁
         pad       [128]byte
    }
##### 1.2 Pool
* 定义：管理多个P/poolLocal
* 结构：

Pool

    type Pool struct {
         local       unsafe.Pointer   //[P]poolLoacl数组指针
         localsize   unitptr          //数组内poolLocal的数量
         New func()  interface{}      //新建对象函数
    }
##### 1.3 func (p *Pool) pin() *poolLocal {}
    返回与当前P绑定的poolLocal对象
##### 1.4 func (p *Pool) pinSlow() *poolLocal {}
     1.3返回无结果时，全局加锁，重新分配数组内存，添加到全局列表，返回需要的poolLocal
### 二. 操作
##### 2.1 Get获取缓存对象
1. 从本地poolLocal可以获取到缓存对象时Get()
   * 优先从private选择
   * 否则，加锁，从share区域选择
2. 本地获取失败时，尝试从其他poolLocal借调/惯偷一个过来getSlow()
   * 获取目标poolLocal，且确保不是自身
   * 对目标poolLocal加锁，以便访问其share区域
   * 偷取一个缓存对象
3. 实在不行，调用New函数新建 
##### 2.2 Put归还缓存对象
1. 获取poolLocal
2. 优先放入private
3. 否则放入share
### 三. 清理
    垃圾回收 ---> 清除缓存池 ---> 清理allPools/Pool/poolLocal/private+share
    
