## 内存分配
### 一. 概述
* 内存块
   * span：由多个地址连续的页/page组成的大块内存
   * object：1个span被切成n个小块/object，每个小块可存储一个对象
   * **1 span = n object, 1 object 存储 1 对象**
   * **span面向内部管理，object面向对象分配**
   * span结构：
       
   span
     
      type mspan struct {
           next      *mspan     //双向链表 下一个span
           prev      *mspan     //前一个span
           start     pageID     //起始序号
           npages    uintptr    //页数
           freelist  gclinkptr  //待分配object链表 
      }

*  管理组件
   * cache：每个运行期工作线程都绑定一个cache，用于无锁分配object
   * central：为所有cache提供切分好的后备span资源
   * heap：管理闲置span，与操作系统交互**申请/归还**内存
*  图解：

   *由下往上并随箭头方向解读*
  
    运行期线程  <-绑定-> cache -分配-> object---小块span，面向内存分配  -分配-> 分配对象 
                         |            |
                       提供span      切分
                         |            |
                       central -->   span----页为基本单位的大块内存
                         |
                       管理central的闲置span
                         |
    操作系统 <-内存交互-  heap 

---
### 二. 分配流程（对照上图）
  1. 计算待分配对象对应的规格（size class）
  2. 从cache.alloc数组找对应规格的span
  3. 从span.freelist链表提取可用object
  4. 若span.freelist为空，从central获取新的span
  5. 若central.nonempty为空，从heap.free(<=127页)/freelarge（>127页/1MB）获取span，并切分成object链表
  6. 若heap没有大小合适的闲置span，向操作系统申请新内存块
  
---
### 三. 释放流程：
  * 大对象：直接从heap分配和回收
  * 小对象：
     1. 将标记为可回收的object交还给所属span.freelist
     2. 该span被放回central，可供任意cache重新获取使用
     3. 如span已收回全部object，则将其交还给heap，以便重新切分复用
     4. 定期扫描heap里长时间闲置的span，释放其占有的内存