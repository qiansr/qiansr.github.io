---
title: Lisp学习指南
date: 2017-12-10 11:08:12
tags: [Lisp, 函数式]
categories: Lisp
---


# 开发环境

## 开发环境：

SBCL  
Emacs  
Slime：在emacs里面帮助进行common lisp开发的扩展  
quicklisp：Common Lisp的包管理工具


## 安装步骤：

### 安装emacs
这个不用多讲了。从GNU的网站能下载到不同平台的版本。

### 安装SBCL
	brew install sbcl       （不支持左右箭头移动光标. 安装rlwrap支持）
	brew install rlwrap   （命令行输入交互增强工具, 支持方向键和历史命令）
	rlwrap sbcl		（启动sbcl命令）

sbcl被安装到/usr/local/bin目录中(记住此地址，后面配置有用)


### 安装quicklisp
	$ curl -O http://beta.quicklisp.org/quicklisp.lisp
	$ sbcl --load quicklisp.lisp
	(quicklisp-quickstart:install)
	(ql:add-to-init-file)
	(ql:quickload "quicklisp-slime-helper")

安装教程：http://www.quicklisp.org/beta/#installation
查找可用的包的信息，可以到cliki上去搜。

### 安装slime
在sbcl里面，运行：

	(ql:quickload "quicklisp-slime-helper")  

根据提示修改Emacs的配置文件。  

	cd ~/ && vi .emacs
	添加下面两句
	(load (expand-file-name "~/quicklisp/slime-helper.el"))
	(setq inferior-lisp-program “/usr/local/bin/sbcl”)  ;你的sbcl路径  

或者下载slime,地址 http://common-lisp.net/project/slime/#downloading  
把slime文件夹copy到了~/.emacs.d/目录中  
配置.emacs配置文件：

	(setq inferior-lisp-program "/usr/local/bin/sbcl")  
	(add-to-list 'load-path "~/.emacs.d/slime/")  
	(require 'slime)  
	(slime-setup)  

做完上面的步骤以后，打开emacs，按M-x，输入slime  
这篇文章也有助于了解Slime: http://www.open-open.com/lib/view/open1400054028504.html



# Lisp 基础语法
## 表达式：
表达式或是一个原子，或是一个由零个或多个表达式组成的表(list)。  
表达式之间用空格分开，放入一对括号中。  
在算术中，表达式 1 + 1 得出值2。  
Lisp表达式也有值，如果表达式e得出 值v，我们说e返回v。  
如果一个表达式是表，我们称第一个元素为操作符，其余的元素为自变量。  
七个原始操作符：quote，atom，eq，car，cdr，cons，cond  

通过引 用(quote)一个表,我们避免它被求值. 一个未被引用的表作为自变量传给象  
atom这样的操作符将被视为代码:

	(atom (atom 'a))
	=> T
反之一个被引用的表仅被视为表, 在此例中就是有两个元素的表:
	
	(atom '(atom 'a))
	=> NIL



## 表与列表操作
Lisp的全名叫“表处理语言”，LISt Procesor 。简单说来，用小括号括起来的表达式式就叫表。而表里面的东西，就是原子，表里不仅可以包含原子，也可以包含另一个表。也就是说表可以嵌套。最小的表就是空表 ( )。  
在lisp中程序和数据都是用 表 来表示。  
在lisp中 T 表示逻辑真；NIL表示逻辑假，同时也是空表。  
Lisp会对所有的表求值，如果想使用表本身 (作为数据), 需在表前加 ‘ 操作符 (单引号)。

	'(+ 1 2)   			
	=> (+ 1 2)

这次解译器不对这个表其求值了，而是原来这个表本身。  
在所有的表中，第一个原子总是函数，代表操作、指令、命令。而之后原子（或表）是参数，意即对操作的说明

### 操作表
car: 				取出表的第一个元素并返回该元素（同first）  
last:				返回最后一个元素的表  
cdr:				返回除第一个元素之外的表（同rest）

	(car '((1 2) 3))   	=>（1 2）
	(cdr '(+ 1 2 3))    	=>  (1 2 3)   	#取出函数的参数
	(cdr '(1))  			=>  NIL
	(cadr '(1 2 3)) 		=>  2           #等价(car (cdr (cdr '(1 2 3))))

其它:cadr	or	caddr

### 构造表
**cons 函数**:  
连接一个元素与一个表，接受两个参数，参数顺序不可颠倒。第二个参数为列表时，才能返回一个列表。cons 的作用是将两棵树连接成一棵树。

	(cons 1 '(2 3))					=> (1 2 3) 
	(cons 2 3)						=> (2 . 3)  
	(cons 1 (cons 2 '(3)))			=> (1 2 3)	 连接三个或以上的元素
	(cons 1 (cons 2 (cons 3 nil)))	=> (1 2 3)
	(cons 3 nil)					=> (3)

**为什么(cons 2 3) 返回 (2 . 3)呢？中间有一个 “.”呢？**

>这是因为表实际上是一个树（二叉树），在S表达式中， 二叉树表示为 (Left . Right)。
如果左支是表，成为形式：((List) . Right)
如果右支是表，表示为 (Left . (List)) ，此时点可以省略，写成(Left List)  
`'(3 . (2 3))  => (3 2 3)`
>这就是为什么cdr操作符会取出除第一个外的所有元素，因为它实质是取二叉树的右支。

**append 函数**:  
它会把最外一层括号去掉，然后连接

	(append '(3 3) '(4 4))  		=>  (3 3 4 4)  连接两个表
	(append '((3)) '(4 4))   		=>  ((3) 4 4)

**list 函数**:
list 函数将所有的参数放入一个表中并返回

	(list 1 1 1 1)  				=>  (1 1 1 1)  返回包含所有参数的表
	(list '(2 3) '(2) 1 2)			=>  ((2 3) (2) 1 2)

构造函数 cons带有两个参数：一个原子和一个列表。cons 将该原子作为第一个元素添加到该列表。如果对 nil 调用 cons，Lisp 将 nil 作为空列表对待，并构建一个含一个元素的列表。append 连接两个列表。list 包含一个由所有参数组成的列表


## 原子和值
**原子**
可以是任何数或者字母排列。空表就是原子NIL。  

	'sdf  => SDF   	原子前面加一个引用符（单引号），返回这个原子本身

**atom 运算符**  
判断一个元素是不是原子  

	(atom 'a)		=> T    	a 是一个原子
	(atom '(3))		=> NIL	(3) 是一个列表而不是原子

**setq 运算符**
绑定一个变量

	(setq a 5) 		=> 5
	a  => 5
setq的意义就是赋值并且将此值返回。就是说表达式(setq a 5)的值是5  
我们可以接着

	(setq a 6)		=> 6
	(cons a '(3))  		=> (6 3)
	(setq a 'b)		=> B
	(cons a '(3))		=> (B 3)
	(setq a '(1 2 3))	=> (1 2 3)
	(cdr a)			=> (2 3)


## 函数

### 函数定义

	(defun 函数名 (参数列表)
	  (first (rest lst))；执行体
	)
defun 用来定义函数。第一个参数是函数名，第二个参数是参数列表，第三个参数是希望执行的代码  
Lisp 所有代码都表述为列表。可以像操纵其他任何数据一样操纵应用程序。

### 断言函数
**atom 函数**
用来判断一个表达式是不是原子

	(atom (+ 1 1))		=> T
	(atom '(3))		=> NIL
因为2是原子，而（3）是个表。

**null 函数**
NULL函数用来判断表达式的值是不是NIL。

	(null nil)			=> T
	(null (car '(3)))		=> NIL

**equal 函数**
用来判断两个表达式的值是否完全相等  

	(equal 's 's)		=> T
	(equal '(s) '(s))		=> T

### 高阶函数
在 Lisp 中，由于函数和列表没有任何区别，高阶函数也就非常简单。
高阶函数的最常见用法或许是 lambda 表达式，这是闭包的 Lisp 版。lambda 函数是用于将高阶函数传入 Lisp 函数的函数定义。
例如，下列lambda 表达式计算了两个整数的和：
(setf total '(lambda (a b) (+ a b)))
(LAMBDA (A B) (+ A B))
 total
(LAMBDA (A B) (+ A B))
(apply total '(101 102))
203

如果使用过高阶函数或闭包，那么可能更容易理解清单 10 中的代码。第一行代码定义了一个 lambda   

表达式并将其和 total 符号绑定到一起。第二行代码仅显示了这个和 total 绑定到一起的 lambda 表达式。最终，最后一个表达式对包含 (101 102) 的列表应用这个 lambda 表达式。
高阶函数提供比面向对象概念更高层次的抽象。可以用它们来更简洁清晰地表达想法。编程的至高境界就是在不牺牲可读性或性能的前提下，用更少的代码提供更强大更灵活的功能。高阶函数能实现所有这些要求。  

Lisp 还有两种类型的高阶函数。其中功能最强大的可能是宏。宏为后面的执行定义 Lisp 对象。可以将宏看作代码模板。请参考清单 11 中的示例：
清单 11. 宏

	(defmacro times_two (x) (* 2 x))
	TIMES_TWO
	 
	(setf a 4)
	4
	 
	(times_two a)
	8

这个示例应该分为两个阶段进行阅读。第一次赋值定义了宏 times_two。在第二个阶段（称为宏扩展）中，在对 a 求值之前，将 a 扩展为 (* 2 a)。该模板中这项延迟求值方式使宏的功能非常强大。Lisp 语言本身的许多功能都是基于宏的。


## 条件结构
**Cond 函数：**

	(cond 分支列表1 分支列表2 分支列表3 ... )
分支列表的构成： (条件p 值e)  
Cond 将对每一个“条件p”求值，如果为NIL，就接着求下一个，如果为真，就返回相应的“值e”，如果没有一个真值，cond操作符返回nil。  

**if 函数：**

	(if 判断表达式 真值时的返回值 假值时的返回值)

eg：计算两个整数中的最大值

	(defun my_max (x y)
	  (if (> x y) x y)
	)

## 递归
**递归计算列表的总和:**  

	(defun total (x)
	  (if (null x)
	    0
	    (+ (first x) (total (rest x)))
	  )
	)
	(total '(1 5 1))   =>  7

total 函数将列表当作单个的参数。第一个 if 语句在列表为空的情况下中断递归，返回零值。否则，该函数将第一个元素添加到列表其余部分的总和。现在应该明白如此构建 first 和 rest 的原因。first 能够去除列表的第一个元素，rest 简化了将尾部递归应用于列表其余部分的过程。

**递归计算列表的长度:**

	(defun len (x) (cond ((null x) 0) (t (+ (len (cdr x)) 1))))
	(len '(a b c d)) => 4

len用来计算一个表x的长（即元素个数）度  
递归式是(len (cdr x)) ，终结条件是(null x)为真


**trace函数**
用来跟踪函数调用的情况

	(trace len)
	(len '(a b c d)) 
	=>
	0: (LEN (A B C D))
	    1: (LEN (B C D))
	      2: (LEN (C D))
	        3: (LEN (D))
	          4: (LEN NIL)
	          4: LEN returned 0
	        3: LEN returned 1
	      2: LEN returned 2
	    1: LEN returned 3
	0: LEN returned 4


## 基本操作符
7个基本操作符对应7大公理，任何其他函数都可以由其定义。也就是说，7个基本操作符包含了Lisp的所有语义。  
**这7个基本操作符是：**
1. Quote
2. Atom
3. Eq
4. Car
5. Cdr
6. Cons
7. Cond

下面的函数系统Lisp都有提供，我们也可以用7个操作符函数重新实现一遍。

**NULL函数**  
NULL函数用于检测表是否为空，或者元素是否为nil。  

	(defun null2 (x) (cond ((equal x nil) t) (t nil))))

解释：如果参数与nil相等，就返回t，否则返回nil。这和逻辑学上的not函数是一致的（但null函数的应用范围更广，因为它可以应用于表）。

**And函数**

	(defun and2 (x y) (cond ((equal x nil) nil) ((not (equal y nil)) t) (t nil)))

**Or函数**

	(defun or2 (x y) (cond ((equal x t) t) ((equal y t) t)))

**Last 函数**

	(defun last2 (x) (cond ((equal (cdr x) nil) x) (t (last2 (cdr x)))))

**Length函数**  
下面讲如何计算一个表x的长（即元素个数）度。

	(defun len (x) (cond ((null x) 0) (t (+ (len (cdr x)) 1))))
递归式是(len (cdr x)) ，终结条件是(null x)为真。

**Append函数**  
设参数形式是x和y。很容易分析出来，递归式是(cons (car x) (append2 (cdr x) y))，终结条件是当x为NIL时，返回y。

	(defun append2 (x y) (cond ((eq x nil) y) (t (cons (car x) (append2 (cdr x) y)))))

**Equal函数**  
设参数形式是x和y。很容易分析出来，递归式是(equal (cdr x) (cdr y))，递归条件是(equal (car x) (car y))，终止条件是(equal (cdr x) nil)或者(equal (cdr y) nil)或者((atom x) (equal x y))

	(defun equal2 (x y) 
	  (cond 
	    ((null x) (not y))
	    ((null y) (not x))
	    ((atom x) (eq x y))
	    ((atom y) (eq x y))
	    ((not (equal2  (car x) (car y))) nil)
	    (t (equal2 (cdr x) (cdr y)))
	  )
	)
代码解释：  

`((null x) (not y))`  
首先，如果x为空，说明遇到了x列表的末尾，这时检测y列表是否也到了，如果到了（此时我们知道之前的元素都相等），那么返回真，否则返回假。  

`((null y) (not x))`  

如果y到了末尾，一样处理。  
`((atom x) (eq x y))`  
如果x是一个原子，说明函数是从`(equal2 (car x) (car y))`字句进入的，且`(car x)`的结果为原子。这时函数就可以结束了，返回x=y的结果。  
`((atom y) (eq x y))`  
如果y是一个原子，说明函数是从`(equal2 (car x) (car y))`字句进入的，且`(car y)`的结果为原子。这时函数就可以结束了，返回x=y的结果。  
`(t (equal2 (cdr x) (cdr y)))`  
否则的情况，我们就递归。  
总结，大家可以发现，其实这个函数的递归路径有两个。  

**If函数**   
用cond可以实现if函数。实际上，在类c语言中，if语句强调的是程序的走向，但在Lisp中，程序的走向可以忽略（从某种意义上），而强调的是返回值。  

	 (defun if2 (p e1 e2)
	 (cond (p e1) (t e2))
	 )


## 其他 

	#'		组合符号  等价函数 function

# Lisp开发Web程序
用lisp可以开发出一套“子语言”，用于生成html。现成的成熟框架，可以参考cl-who。

可以把lisp代码编译成javascript。你用lisp编写的程序，到了html里，就成了javascript。可以看一下parenscript(http://www.cliki.net/parenscript)  

cliki上搜一下web framework，你可以得到一大堆。你可以试一下cl-weblocks。阅读一下它的文档，你可以看到很多厉害的特性，忍不住会上手试一下！http://weblocks-framework.info 上面有不错的文档。有的文档资料可能旧了点，但能够说清楚。  

## 使用Weblocks编写Web应用

安装weblocks

	(ql:quickload :weblocks)

安装demo程序

	(ql:quickload :weblocks-demo)

运行

	(weblocks:start-weblocks)    ;可以在8080端口启动一个http服务。

运行示例

	(weblocks-demo:start-weblocks-demo :port 3455)

访问：http://localhost:3455/weblocks-demo  
(如果发现这样的报警，可以忽略，不影响：An instance of WEBLOCKS-DEMO with name weblocks-demo is already running, ignoring start request)  

创建自己的应用

	(wop:make-app 'NAME "DIR")    ;建立名称为Name的应用，放在DIR目录下

例如：(wop:make-app 'firstapp "home/richard/lisp")  
这样就会在“home/richard/lisp”目录建立一个firstapp子目录，里面有几个必要的目录和模板程序。  

加载和修改自己的应用
要能在开发环境加载和修改自己的应用，需要能够找到firstapp。这需要了解一下asdf的配置。asdf是lisp的模块依赖的管理工具。具体可以参考：  

http://www.common-lisp.net/project/asdf/  
http://basiccoder.com/constructing-common-lisp-package-by-asdf.html  
http://blog.csdn.net/xiaojianpitt/article/details/7727152  


我的配置：  
建立目录：/home/richard/.config/common-lisp/source-registry.conf.d  
在里面建两个配置文件：  
01practicals.conf  
内容： (:tree "/home/richard/practicals-1.0.3/")  
这样就可以随时加载执行《实用commonlisp编程》里面的例子了。  
比如：(ql:quickload :chapter31)就可以加载第31章的例子。  
02richard.conf 指向我自己的工作目录。  
内容：(:tree "/home/richard/lisp/")。  
这样就可以找到firstapp。用(ql:quickload :firstapp)来加载和修改。  

加载后，运行

	(in-package :firstapp)  切换到:firstapp包；
	(start-firstapp :port 3456)  启动应用，在3456端口监听


学习资料推荐  
Lisp的本质  
http://www.cnblogs.com/Leap-abead/articles/762180.html  
Lisp之根源（The roots of Lisp）  
中文版：http://daiyuwen.freeshell.org/gb/rol/roots_of_lisp.html    
Lisp之美  
https://www.ibm.com/developerworks/cn/java/j-cb02067.html  
Common Lisp 初学者快速入门指导  
https://my.oschina.net/freeblues/blog/131557#1.2  
在Mac下搭建Common Lisp开发环境(Emacs)  
http://it.taocms.org/06/954.htm  



