### 第五章 引用类型

本章内容：使用对象、创建并操作数组、理解基本的JavaScript类型、使用基本类型和基本包装类型

引用类型是一种数据结构，用于将数据和功能组织在一起。它也常常被叫做类，但是不太妥当。

#### 5.1 Object类型

创建object实例方式有两种，第一种使用new 操作符 后面跟object构造函数：

var person = new Object();

person.name = “Nicholas”;

person.age = 29;

另一种方式是使用对象字面量表示法。

var person = {

​		name : “Nicholas”,

​		age : 29

};

#### 5.2 Array类型

ECMAScript数组的每一项可以保存任何类型的数据。且数组的大小可以动态调整，可以随着数据添加自动增长以容纳新增。

数组的两种创建方法：

var colors = new Array();

预先知道数组要保存的数量，也可以给构造函数传递数量。

var colors = new Array(20);



第二种基本方式是字面量表示法:

var colors = [“red”,“blue”,“green”];

var names = [];

不要创建数组时在方括号中添加值与空白值混合【1,2， 】，这样会在有些浏览器有问题。

