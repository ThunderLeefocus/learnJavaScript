### 第八章

本章内容：理解window对象——BOM核心、控制窗口、框架和弹出窗口

利用Location对象中的页面信息、使用navigator对象了解浏览器

#### 8.1 window 对象

BOM的核心对象是window，它表示浏览器的一个实例。浏览器中，window对象有双重角色，既是javascript访问浏览器窗口的一个接口，又是ECMAScript规定的Global对象。网页中定义的任意对象、变量和函数，都是以window作为其Global对象，有权访问parseInt（）等方法。



##### 8.1.1 全局作用域

由于window对象同时扮演ECMAScript中Global对象的角色。所有在全局作用域中声明的变量、函数都会成为window对象的属性和方法。

全局变量不能够通过delete操作符删除，而直接在window对象上的定义的属性可以：

```
var age  = 29;
window.color = "red";
//IE<9抛出错误，其他所有浏览器都返回false
delete window.age;

//IE<9抛出错误，其他所有浏览器都返回true。
delete window.color;

alert(window.age);
alert(window.color);//undefined
```

##### 8.1.2 窗口关系及框架

如果页面包含框架，则每个框架都有自己的window对象，并保存在frames集合中。

与框架有关的最后一个对象是self，它始终指向window；实际上，self和window对象可以互换使用。引入self 对象的目的只是为了top和parent对象对应起来。它不格外包含其他值。