---
title: Java学习笔记
date: 2018-01-11 01:01:06
tags: [java]
categories: Java
---

# 数据结构

**8种基本数据类型**

|  数据类型 | type  |  包装类 |
|---|---|---|
|基本类型| byte	|java.lang.Byte|
|基本类型| short	|java.lang.Short|
|基本类型| int	|java.lang.Integer|
|基本类型| long	|java.lang.Long|
|基本类型| float	|java.lang.Float|
|基本类型| double	|java.lang.Double|
|基本类型| char	|java.lang.Character|
|基本类型| boolean|java.lang.Boolean

引用类型:   Class,Collection集合类  
自定义类




# 抽象类和接口

## 抽象类
1. 抽象类
包含一个抽象方法的类就是抽象类
2. 抽象方法
声明而未被实现的方法，抽象方法必须使用abstract关键词字声明
```
    public abstract class People {  //关键词abstract，声明该类为抽象类
        public int age;
        public void Num() {
        }
        public abstract Name(); //声明该方法为抽象方法
    }　　
```

3. 抽象类被子类继承，子类（如果不是抽象类）必须重写抽象类中的所有抽象方法
4. 抽象类不能被直接实例化，要通过其子类进行实例化
5. 只要包含一个抽象方法的抽象类，该方法必须要定义成抽象类，不管是否还包含有其他方法。
6. 子类中的抽象方法不能与父类的抽象方法同名。
7. abstract不能与final并列修饰同一个类。
8. abstract 不能与private、static、final或native并列修饰同一个方法。
 
## 接口
1. 接口，英文称作interface，在软件工程中，接口泛指供别人调用的方法或者函数。在Java中它是对行为的抽象。
2. 接口中可以定义 变量和方法。
3. 接口中的变量会被隐式地指定为public static final变量（并且只能是public static final变量，用private修饰会报编译错误）。
4. 接口中的方法会被隐式地指定为public abstract方法且只能是public abstract方法（用其他关键字，比如private、protected、static、 final等修饰会报编译错误），
5. 接口中所有的方法不能有具体的实现，也就是说，接口中的方法必须都是抽象方法。

**接口的格式**

    interface interfaceName{
        全局常量
        抽象方法
    }
     
    class A  implements Interface1,Interface2,[....]{ 
        ...  接口的实现使用关键字implements，而且接口是可以多实现的
    }
     
    class A extends Abs implements Inter1,Inter2{ //Abs是一个抽象类
        ...一个类可以同时继承抽象类和接口
    }
     
    interface Inter implements Inter1,Inter2{ //Inter、Inter1、Inter2都为接口
        ...接口能通过extends关键字继承多个接口
    }


## 抽象类和接口区别

### 语法层次
    public abstract class People {  //关键词abstract，声明该类为抽象类
        void Num();　　　　　　
        abstract void Name(); 　　　//声明该方法为抽象方法
    }
     
    Interface Person {
    　　void Num();
    　　void Name();
    }　
抽象类方式中，抽象类可以拥有任意范围的成员数据，同时也可以拥有自己的非抽象方法，
但是接口方式中，它仅能够有静态、不能修改的成员数据（但是我们一般是不会在接口中使用成员数据），同时它所有的方法都必须是抽象的。  
在某种程度上来说，接口是抽象类的特殊化。对子类而言，它只能继承一个抽象类（这是java为了数据安全而考虑的），但是却可以实现多个接口。

### 设计层次
1、 抽象层次不同  
抽象类是对类抽象。  
接口是对行为的抽象。  
抽象类是对整个类整体进行抽象，包括属性、行为。  
接口却是对类局部（行为）进行抽象。

2、 跨域不同  
抽象类所跨域的是具有相似特点的类，而接口却可以跨域不同的类。我们知道抽象类是从子类中发现公共部分，然后泛化成抽象类，子类继承该父类即可。但是接口不同。实现它的子类可以不存在任何关系，共同之处。例如猫、狗可以抽象成一个动物类抽象类，具备叫的方法。鸟、飞机可以实现飞Fly接口，具备飞的行为，这里我们总不能将鸟、飞机共用一个父类吧！所以说抽象类所体现的是一种继承关系，要想使得继承关系合理，父类和派生类之间必须存在"is-a" 关系，即父类和派生类在概念本质上应该是相同的。对于接口则不然，并不要求接口的实现者和接口定义在概念本质上是一致的， 仅仅是实现了接口定义的契约而已。

3、 设计层次不同  
抽象类是自下而上来设计的，我们要先知道子类才能抽象出父类。  
接口不需要知道子类的存在，只需要定义一个规则即可，至于什么子类、什么时候怎么实现它一概不知。  

比如我们只有一个猫类在这里，如果你这是就抽象成一个动物类，是不是设计有点儿过度？我们起码要有两个动物类，猫、狗在这里，我们在抽象他们的共同点形成动物抽象类吧！所以说抽象类往往都是通过重构而来的！  
但是接口就不同，比如说飞，我们根本就不知道会有什么东西来实现这个飞接口，怎么实现也不得而知，我们要做的就是事前定义好飞的行为接口。所以说抽象类是自底向上抽象而来的，接口是自顶向下设计出来的。

### 总结
抽象类的子类不能再继承其他的类,可以实现多个接口.因为java是单继承的.  
如果说目前有一个类已经继承(extends)其他类了,如果这个时候又有一个父类出现,那么只能定义为他的父类为接口,不能定义为抽象类  
抽象类中除了能定义抽象方法以外,也可以定义具体的方法,并且可以定义方法体内容.  
接口中是不可以定义具体的方法实现,他只能允许你定义方法但是不能有任何方法体.  
概念上的区别:  
抽象类:如果一个类中没有包含足够的信息来描述一个具体的对象,这样的类就是抽象类。接口是一种特殊的抽象类，可以这么理解,接口是抽象类的一种特殊表现,有自己的一套规范约束在里面.  
实现抽象类和接口的类必须实现其中的所有方法。抽象类中可以有非抽象方法。接口中则不能有实现方法。


### 门和警报的例子
门都有`open( )`和`close( )`两个动作，此时我们可以定义通过抽象类和接口来定义这个抽象概念。

    abstract class Door {
        public abstract void open();
        public abstract void close();
    }
    or：
    interface Door {
        public abstract void open();
        public abstract void close();
    }

给门延伸报警`alarm( )`的功能，那么该如何实现？

**放在抽象类里面：**  
但是这样一来所有继承于这个抽象类的子类都具备了报警功能，但是有的门并不一定具备报警功能；

**放在接口里面：**  
需要用到报警功能的类就需要实现这个接口中的`open( )`和`close( )`，也许这个类根本就不具备`open( )`和`close( )`这两个功能，比如火灾报警器。

从这里可以看出， Door的`open()`、`close()`和`alarm()`根本就属于两个不同范畴内的行为，`open()`和`close()`属于门本身固有的行为特性，而`alarm()`属于延伸的附加行为。

**最好的解决办法：**  
单独将报警设计为一个接口，包含`alarm()`行为。  
Door设计为单独的一个抽象类，包含open和close两种行为。  
再设计一个报警门继承Door类和实现Alarm接口。

    interface Alram {
        void alarm();
    }
     
    abstract class Door {
        void open();
        void close();
    }
     
    class AlarmDoor extends Door implements Alarm {
        void oepn() {
          //....
        }
        void close() {
          //....
        }
        void alarm() {
          //....
        }
    }




## java 中的字符串
Java中String、StringBuffer、StringBuilder和toString的介绍
 
### String类
1、字符串长度——length（）  

    String str = "coder";  
    System.out.print(str.length());
    输出结果：
    5

2、字符串转换数组——toCharArray（）

    String str = "coder";
    char data[] = str.toCharArray(); //调用String类中toCharArray方法
    for (int i = 0; i < data.length; i++){
    System.out.print(data[i]+" "); //加入空格，以示区分
    }
    输出结果：
    c o d e r

3、从字符串中取出指定位置的字符——charAt()

    String str = "coder";
    System.out.print(str.charAt(3));
    输出结果：
    e

4、字符串与byte数组的转换——getBytes()

    String str = "coder";
    byte bytes[] = str.getBytes();
    for (int i = 0; i < bytes.length; i++){
    System.out.print(new String(bytes)+"\t"); //加入换行，以示区分
    }
    输出结果：
    coder
    coder
    coder
    coder
    coder

5、过滤字符串中存在的字符——indexOf() 

    String str = "coder@163.com";
    System.out.print(str.indexOf("@"));
    输出结果：
    5

6、去掉字符串的前后空格——trim()

    String str = " coder@163.com ";
    System.out.print(str.trim());
    输出结果：
    coder

7、从字符串中取出子字符串——subString()  
8、大小写转换——toLowerCase()、toUpperCase()  
9、判断字符串的开头结尾字符——endWith()、startWith()  
10、替换String字符串中的一个字符——replace()
 
## StringBuffer类
1、认识StringBuffer:  
缓冲区、本身也是操作字符串，但是与String不同，StringBuffer是可以更改的。StringBuffer也是一个操作类，所以必须通过实例化进行操作  

2、StringBuffer常用方法：

    append()
    insert()
    replace()
    indexOf()
    举例：
    StringBuffer str = new StringBuffer();
    str.append("coder");
    system.out.print(str.toString());
     
    输出结果：
    coder
 
## StringBuilder类
1、认识StringBuilder:  
一个可变的字符序列，该类被设计作用StringBuffer的一个简易替换，用在字符串缓冲区被单个线程所使用的时候。建议优先考虑该类，速度比StringBuffer要快  
2、但是如果涉及到线程安全方面，建议使用StringBuffer  
3、StringBuilder常用方法：

    append()
    insert()
    replace()
    indexOf()
  
## toString()方法
因为它是Object里面已经有了的方法，而所有类都是继承Object，所以“所有对象都有这个方法”。它通常只是为了方便输出，比如System.out.println(xx)，括号里面的“xx”如果不是String类型的话，就自动调用xx的toString()  
方法。总而言之，它只是sun公司开发java的时候为了方便所有类的字符串操作而特意加入的一个方法。
举例：

    StringBuffer str = new StringBuffer();
    str.append("coder");
    system.out.print(str.toString());
    输出结果：
    coder


## void关键字
void就是空，在方法申明的时候表示该方法没有返回值  
那么java中的void到底是什么类型呢？其实void也有对应的包装类`java.lang.Void`，不过我们无法直接对它们进行操作。 它继承于Object，如下：

    public final class Void extends Object {
        /*
         * The Void class cannot be instantiated.
         */
        private Void() {}
    }
Void类和String类一样 被定义为final，所以不能扩展；另外，它的构造方法被私有化了所以不可实例化  
Void类是一个不可实例化的占位符类，用来保存一个引用代表了Java关键字void的Class对象。
