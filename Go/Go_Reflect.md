## Go_Reflect
### 1.简介
* **反射**：使您能够在运行时探知对象的类型信息和内部结构
* 编译时就知道变量类型的是**静态类型**；运行时才知道一个变量类型的叫做**动态类型**
* **Go**是静态类型语言。每个变量都拥有一个静态类型，这意味着每个变量的类型在编译时都是确定的：int，float32, *AutoType, []byte,  chan []int 诸如此类。
* **python**是动态类型语言，反射类似python的**自省**（在运行时知道自己的类型）
### 2.常用知识点
1 反射操作所需的全部信息来源自**接口变量**，接口变量存储自身类型及实际对象的类型数据。同时将所有传入对象转换为接口类型。

    func TypeOf(i interface{}) Type
    func ValueOf(i interface{}) Value

2 **reflect.TypeOf() 与 reflect.Kind()**

    type X int
    
    func main(){
        var a X = 100
        t := reflect.TypeOf(a)  //真实类型，X
        fmt.Println(t.Kind())   //基础类型，int
    }

3 **构造**基础复合类型

    func main(){
       a := reflect.Arrayof(10, reflect.TypeOf(byte(0)))
       b := reflect.MapOf(reflect.TypeOf(""), reflect.TypeOf(0))
    }

4 **Elem()**返回指针、数组、切片、字典value、通道的**基类型**

    func main(){
        fmt.Println(reflect.TypeOf(map[string]int{}).Elem())   // int
        fmt.Println(reflect.TypeOf([]int32{}).Elem())          // int32
    }

5 只有获取**结构体指针的基类型**后，才能**遍历**其字段

    type user struct{
         name string
         age  int
    }
    
    type manage struct{
         user
         title string
    }
    
    func main() {
        var m manager
        t := reflect.TypeOf(&m)         //获取结构体指针
        
        if t.Kind() == reflect.Ptr {    
            t = t.Elem()                //获取指针的基类型
        }
       
        for i := 0; i < t.Numfield(); i++ {
            f := t.Field(i)
            fmt.Println(f.name, f.Type, f.Offset)
            if f.Anonymous {
                for x := 0; x < f.Type.NumField(); x++ {
                    af := f.Type.Field(x)
                    fmt.Println(" ", af.Name, af.Type)
                }
            }
        }
    }
    
    输出：
        user main.user 0
             name  string
             age   int
        title  string

6 可用reflect提取struct tag，自带分解

    type user struct{
         name string 'field:"name" type:"varchar(50)"'
         age  int  'field:"age" type:"int"'
    }

    func main(){
         var u user
         t := reflect.TypeOf(u)
         for i := 0; i < t.NumField(); i++{         //结构体遍历过程
             f := t.Field(i)
             fmt.Printf("%s: %s %s\n",f.name,f.Tag.Get("field"),f.Tag.Get("type"))      //识记
         }
    }
    
    输出：
        name: name varchar(50)
        age: age  int

补充示例：

    package main

    import (
         "fmt"
         "reflect"
    )

    func Print(str ...string) {
         fmt.Println("str=", str)
    }

    func main() {
         valueOf := reflect.ValueOf(Print) 
         i := make([]reflect.Value, 3)          //构造基础类型
         i[0] = reflect.ValueOf("1")            //取值
         i[1] = reflect.ValueOf("2")
         i[2] = reflect.ValueOf("3")
         valueOf.Call(i)
         
         
    }