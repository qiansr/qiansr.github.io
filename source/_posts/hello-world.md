---
title: Hello World
categories:
- 前端
tags:
- hexo
---



## Swig语法
{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
Every interaction is both precious and an opportunity to delight.
{% endblockquote %}

## Markdown语法
> Every interaction is both precious and an opportunity to delight.
代码块


## Swig语法
{% codeblock .compact http://underscorejs.org/#compact Underscore.js %}
.compact([0, 1, false, 2, ‘’, 3]);
=> [1, 2, 3]
{% endcodeblock %}

## Markdown语法
```{bash}
.compact([0, 1, false, 2, ‘’, 3]);
=> [1, 2, 3]
```
链接


{% link 粉丝日志 http://blog.fens.me true 粉丝日志 %}

## Markdown语法
[粉丝日志](http://blog.fens.me)
图片，对于本地图片，需要在source目录下面新建一个目录images，然后把图片放到目录中。


## Swig语法
{% img /images/fens.me.png 400 600 这是一张图片 %}

# Markdown语法
![这是一张图片](/images/fens.me.png)


## 公式
$$J\_\alpha(x)=\sum _{m=0}^\infty \frac{(-1)^ m}{m! \, \Gamma (m + \alpha + 1)}{\left({\frac{x}{2}}\right)}^{2 m + \alpha }$$


------
<br/>
<br/>

# 完整的Swig语法
通过下面的命令，就可以创建新文章
```{bash}
D:\workspace\javascript\nodejs-hexo>hexo new 新的开始
[info] File created at D:\workspace\javascript\nodejs-hexo\source\_posts\新的开始.md
```

感觉非常好。


## 引用
{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
Every interaction is both precious and an opportunity to delight.
{% endblockquote %}

## 代码块
{% codeblock .compact http://underscorejs.org/#compact Underscore.js %}
.compact([0, 1, false, 2, ‘’, 3]);
=> [1, 2, 3]
{% endcodeblock %}

## 链接
{% link 粉丝日志 http://blog.fens.me true 粉丝日志 %}

## 图片
{% img /images/fens.me.png 400 600 这是一张图片 %}

