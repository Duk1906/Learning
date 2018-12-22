## Go_Interface
### 一.简介
1. 定义：是一种类型(接口类型)，多个方法的集合，**关心的是做什么，而不是怎么做**
2. 说点其他
  * 函数：面向过程，输入输出
  * 方法：面向对象，特殊的函数(输入变成了对象)
> Interface是一组Method的组合，可以通过Interface来定义**对象的一组行为**。如果某个对象实现了某个接口的所有方法，就表示它实现了该借口，无需显式地在该类型上添加接口说明。Interface里面没有其他类型变量，而且**Method只用定义原型，不用实现**
### 二.实例

实现一个动物的接口：

    package main
    import "fmt"

    // 定义一个 Animal接口
    type Animal interface {
         Eat()
         Talk()
         Run()
    }

    // Dog 实现了Animal的所有方法，就可以说Dog实现了Animal这个接口
    type Dog struct {
         name string
    }

    func (d *Dog) Eat() {
         fmt.Printf("%s is Eating...\n", d.name)
    }

    func (d *Dog) Talk() {
         fmt.Printf("%s is Talking...\n", d.name)
    }

    func (d *Dog) Run() {
         fmt.Printf("%s is Running...\n", d.name)
    }

    func main() {
         var dog = &Dog{
             name: "WangCai",
         }

         var a Animal
         a = dog
         a.Run()
         a.Eat()
         a.Talk()
    }
