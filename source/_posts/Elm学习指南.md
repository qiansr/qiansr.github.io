---
title: Elm学习指南
date: 2017-09-10 15:47:10
tags: [函数式,elm.js]
categories: 函数式编程
---


# Elm基础语法

## 常用数据结构
列表List	： 只能容纳单一类型元素,是类型的容器而不是类型，List String才是类型  
元祖Tuples：可为不同类型，固定长度小于10  
Records	：  具有键值对的数据结构

### List （列表）
	list 方法
	List.drop 1 [1,2,3,4]   —  [1,2,3]
	List.length [1,2,3,4]   — 4
	List.head [1,2,3,4]  	— Just 1 : Maybe.Maybe number
	List.tail [1,2,3,4] 	— Just [2,3,4] : Maybe.Maybe (List number)
	List.reverse [1,2,3,4]	— [4,3,2,1] : List number   		反序
	List.sort [1,4,3,2]	— [1,2,3,4] : List number   		排序 
	List.maximum a	    — Just 4 : Maybe.Maybe number	最大值
	List.minimum a		— Just 1 : Maybe.Maybe number	最小值	
	0::[1,2,3,4] 		— [0,1,2,3,4] : List number		“::”向列表添加元素0 
	[-1,-2]++[0,1,2,3,4]	— [-1,-2,0,1,2,3,4] : List number  	“++”拼接列表

### tuples （元组）

### records  （记录）
	point = { x = 3, y = 4 }                -- create a record
	point.x                                 -- 访问字段
	List.map .x [point,{x=0,y=0}]           -- field access function
	{ point | x = 6 }                            -- update 字段
	{ point | x = point.x + 1, y = point.y + 1}  -- update 多个字段
	dist {x,y} = sqrt (x^2 + y^2)                -- pattern matching on fields

	type alias Location =    { line : Int , column : Int}    -- type aliases for records

	{x=3,y=7}.x  or .y {x=3,y=7} ／／.y为一个函数
	bill = {name = “cyq”, age = 20}
	bill2 = {bill | name = “222”}  	copy一份bill — { name = “222”, age = 20 } : { age : number, name : String }

* 你不可以使用不存在的field.
* field不可為 undefined 或是 null.
* 你不可以使用 this 或 self 等關鍵字來創造遞迴的 records


## 模式匹配
有点类似es6的desttructuring


## 中缀运算符 （可自定义）
您可以创建自定义中缀运算符。 优先级从0到9，其中9是最紧密的。默认优先级为9，默认 关联性为左。您可以自行设置，但不能覆盖内置运算符。


## 模块
	module MyModule exposing (..)
	-- qualified imports
	import List                    -- List.map, List.foldl
	import List as L               -- L.map, L.foldl

	-- open imports
	import List exposing (..)               -- map, foldl, concat, ...
	import List exposing ( map, foldl )     -- map, foldl

优先进口合格。模块名称必须与其文件名匹配，因此模块Parser.Utils需要在文件中Parser/Utils.elm。

## 类型注释
一些常见的类型：Int，Float，String，Bool

## 型別別名 (type alias)
有時候，單純的基本型別可能無法完全詮釋我們想要表達的意思，也有可能是資料結構太複雜，我們想用一個簡單的名稱稱呼它，這時型別別名就可派上用場。

	type alias Name = String
	type alias Age = Int
	type alias Person =
	    { name: String
	    , age: Int
	    }

	createPerson: Name -> Age -> Person
	createPerson name age =
	    { name = name
	    , age = age
	    }
`type alias Name = String` 的意思可以翻譯為 將 Name 設定為 String 的另一個別名。如此一來，我們就可以像這樣定義:
`createPerson: Name -> Age -> Person`
而不是
`createPerson: String -> Int -> Person`

### 自定义类型：（建立自創型別與 Union Type）
自創型別的方法如下：
`type Directions = Up | Down | Left | Right`  
`type Maybe a = Nothing | Just a`
我們可以創造兩種不同款式的型別，第一種像是上例的 Directions 比較直觀簡單，在這型別下只會有四種可能的值，就是我們定義的 Up，Down，Left，Right。在使用上的話我們可以這麼做：

	convertDirectionsToInt: Directions -> Int
	convertDirectionsToInt dir =
	    case dir of
	        Up -> 1
	        Down -> 2
	        Left -> 3
	        Right -> 4

	convertDirectionsToInt Up -- 1

## Union Types  
用来表示一组可能的值，每个值叫做一个Tag  
指的是我們可以將其它已存在的型別組合進我們自創的型別中。範例中我們建立了一個叫 Maybe 的型別，後面跟著的 a 是所謂的 型別變數，再更後面則是型別的值。我們可以自由在型別變數中帶入其他的型別如下：

	-- 把 String 帶入 a
	maybeString: Maybe String
	maybeString = Just "I am a string"

	-- 把 Int 帶入 a
	maybeInt: Maybe Int
	maybeInt = Just 1

	anotherMaybeInt: Maybe Int
	maybeInt = Nothing
以上三個變數都是屬於 Maybe a 型別，但隨著帶入的 a 不同，型別裡的值也會跟著改變。當然你也可以建立更複雜的 Union Type，例如:
`type Directions a b = Up a | Down b | Left a b | Right`


## javascript 互操作
1. 在HTML中嵌入Elm，
2. 在Elm和JavaScript之间来回发送消息


## 函数
	multiply a b = a*b
	double = multiply 2
	List.map double [1,2,3,4]

	-- 通过匿名函数形式。
	List.map (\a -> a * 2) [1..4] -- [2, 4, 6, 8]

`\counterMsg -> Modify id counterMsg`是Elm中的匿名函数，在Elm中，匿名函数使用\开头紧接着参数，
并在->后书写返回值表达式，形如`\a -> b`。

-- 通过`let...in...`语句来定义来定义一些将要立即使用的值。

	volume {width, height, depth} = let area = width * height in area * depth
	volume { width = 3, height = 2, depth = 7 } -- 42

函数名  参数 = 

## 控制语句
if true then “WHOA” else if true then “n” else “0”

	case alist of
	[]->””
	[x]->””
	x::xs->””



# elm 工程化
	elm-webpack-project 脚手架
	git clone https://github.com/moarwick/elm-webpack-starter 
	rm -rf .git        
	git init   git add .   git commit -m 'first commit'
	npm run reinstall  安装依赖


自己搭建

	－－－－package.json
	 "scripts": {
	    "elm-install": "elm-package install",
	    "build": "elm-make Main.elm --output=build/index.js",
	    "start": "elm-live Main.elm --output=build/index.js --open" 
	  },
	  "devDependencies": {
	    "elm": "^0.18.0",
	    "elm-live": "^2.7.4" //热更新，热加载模块
	  }


	－－－－－elm-package.json
	 "dependencies": {
	        "elm-lang/core": "5.0.0 <= v < 6.0.0",
	        "elm-lang/html": "2.0.0 <= v < 3.0.0",
	        "elm-lang/http": "1.0.0 <= v < 2.0.0",
	        "evancz/elm-markdown": "3.0.1 <= v < 4.0.0"
	  },
	  "elm-version": "0.18.0 <= v < 0.19.0"



	－－－－－－－index.html
	<div id=“main”></div>
	<script type="text/javascript">
	      var node = document.getElementById(‘main’);
	      var app = Elm.Main.embed(node);
	 </script>


	－－－－－
	npm i
	npm run elm-install
	npm run start



# 官网示例库  
git clone https://github.com/evancz/elm-architecture-tutorial.git  
cd elm-architecture-tutorial

**学习资源**  
Elm入门实践系列  
https://segmentfault.com/a/1190000005701562  
初识Elm语言你只需要Y分钟  
https://github.com/Jocs/jocs.github.io/issues/2  
Elm架构教程  
https://segmentfault.com/a/1190000004872909  


 
