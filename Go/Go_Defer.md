## Go延迟_defer
* defer的秘密
* deferproc
* deferreturn
### 一.defer的秘密
##### 1.1秘密
    编译器将defer处理成两个函数调用，deferproc定义一个延迟对象，
    然后在函数结束前通过deferreturn完成最终调用。
##### 1.2结构

    type _defer struct {
         siz      int32      //目标函数参数长度
         started  bool       //启动标识
         sp       unitprt    //调用deferproc时的SP
         pc       unitprt    //调用deferproc时的PC
         fn       *funcval   //参数不确定的函数
         _panic   *_panic    //panic that is running defer
         link     *_defer    //_defer结构体的指针
### 二.deferproc
##### 2.1定义
> 定义一个延迟对象，并初始化结构信息
##### 2.2过程
    调用了newdefer函数，一次性为defer和参数分配空间，
    创建的d（*_defer）被挂到 G. _defer 链表，
    退出前deferreturn从G. _defer链表获取并执行延迟函数
### 三.deferreturn
##### 3.1过程
1. 从G._defer提取延迟对象
2. 对比SP，避免调用其他栈帧的延迟函数
3. 将延迟函数的参数复制到堆栈（memmove）
4. 调整G._defer链表
5. 释放_defer对象，放回缓存（freedefer（d））
6. 执行延迟函数（jmpdefer（））
##### 3.2freedefer（）
    它将_defer放回P.deferpool缓存，当数量超出时会转移部分出sched.deferpool。
    垃圾回收时，chearpools会清理掉sched.deferpool缓存
##### 3.3jmpdefer（）
    实现多个defer延迟调用循环