### 第4章 变量、作用域和内存问题

本章内容：理解基本类型和引用类型的值、理解执行环境、理解垃圾收集、



#### 4.1 基本类型和引用类型的值

ECMAScript 变量可能包含两种不同数据类型的值： 基本类型值 和 引用类型值。

基本类型值指的是简单的数据段，引用类型值 指的是可能由多个值构成的对象。

引用类型的值是保存在内存中的对象。与其他语言不同，JavaScript 不允许直接访问内存中的位置。

##### 4.1.1 动态的属性

```
var person = new Object();
person.name = "Nicholas";
alert(person.name);  //"Nicholas"
```

创建一个对象并保存在变量person中。为对象添加了一个name的属性，并给它赋值。可以通过函数访问到这个新的属性。

但是，我们不能给基本类型的值添加属性，尽管这样做不会导致任何错误。

```
var name = "Nicholas";
name.age = 27;
alert(name.age);  //undefined
```

为字符串name定义了一个属性，并且赋值27，但是访问属性时，属性不见了。



##### 4.1.2 复制变量值

从一个变量向另一个变量复制基本类型值和引用类型值时，也存在不同。如果从一个变量向另一个变量复制基本类型的值，存在变量对象上创建一个新值，然后把该值复制到为新变量分配的位置上。

从一个变量向另一个变量复制引用类型的值，同样也会将存储在变量对象中的值复制一份放到为新变量分配的空间。不同的是这个值的副本实际上是一个指针。而这个指针指向存储在堆中的一个对象。复制结束后，两个变量实际上将引用同一个对象。因此改变其中一个变量，就会影响另外一个。

```
var obj1 = new Object();
var obj2 = obj1;
obj1.name = "Nicholas";
alert(obj2.name);  //"Nicholas"
```



##### 4.1.3 传递参数

所有函数的参数都是按值传递的。把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另一个变量一样。

基本类型值传递如同基本类型变量复制，引用类型值的传递，如同引用类型变量的复制。

```
function addTen(num){
	num += 10;
	return num;
}
var count = 20;
var result = addTen(count);
alert(count); //20,没有变化
alert(result); //30
```

函数addTen() 有一个参数num,而参数实际上是函数的局部变量。调用函数时，变量count作为参数被传递给函数，count的值20，被复制给参数num，以便在addTen（）中使用。函数内部 参数num加10，并不会影响函数外部count的变量。

```
function setName（obj）{
	obj.name = "Nicholas";
}
var person = new Object();
setName(person);
alert(person.name); //"Nicholas"
```





##### 4.1.4 检测类型

typeof操作符是确定一个变量类型的最佳工具。 如果变量是一个对象或是null,typeof 操作符会返回“object”

通常，想知道某个值是什么类型的对象时，用instanceof操作符：

result = variable instanceof constructor 

alert (person instanceof object /Array /RegExp); // 变量person是object/Array /RegExp吗？



#### 4.2 执行环境及作用域

##### 4.2.1 延长作用域链

执行环境的类型总共两种：全局和局部（函数），还有其他办法延长作用域链。

+ try - catch 语句的 catch 块 ；

+ with 语句。

  

两个语句都会在作用域链前段添加一个变量对象。



##### 4.2.2 没有块级作用域

1.声明变量

​	使用var声明的变量会自动被添加到最接近的环境中。在函数内部，最接近的环境就是局部环境；

with 语句中，最接近的环境就是函数环境。 如果初始化变量没有用var 声明，该变量会自动被添加到全局环境。

```
function add(num1,num2){
	var sum = num1 + num2;
	return sum;
}
var result = add(10,20); // 30 
alert（sum）； // sum不是有效变量，会导致错误
```

add（）函数定义了一个sum 的局部变量。变量sum在函数外部访问不到。如果省略掉例子中的 **var**关键字，那么当add()执行完毕，sum也可以访问到：

```
function add(num1,num2){
	sum = num1 + num2; 
	return sum;
}
var result = add(10,20); // 30 
alert（sum）；            // 30
```

!!!**注意：** 在编写JavaScript代码中，不声明而直接初始化变量是错误做法，会导致意外。

2.查询标识符

​	当在某个环境中为了读取或写入而引用一个标识符时，必须通过搜索来确定该标识符实际代表什么。过程从作用域链前端开始，向上逐级查询与给定名字匹配的标识符。

```
var color = "blue";

function getColor(){
	var color = "red";
	return color;
}
alert(getColor()); // "red"
```

#### 4.3 垃圾收集

##### 4.3.1 标记清除

JavaScript最常用的垃圾收集方式是**标记清除（mark-and-sweep）**。

##### 4.3.2 引用计数

另一种不太常见的垃圾收集策略叫做引用计数。

##### 4.3.3 性能问题

##### 4.3.4 管理内存

#### 4.4 小结

JavaScript变量可以用来保存两种类型的值：基本类型和引用类型值。

基本类型有五种：undefined、null、number、Boolean和String。

+ 基本类型值在内容中占据固定大小空间 ，因此被保存在栈内存中；
+ 从一个变量向另一个变量复制基本类型的值，会创建这个值的副本；
+ 引用类型的值是对象，保存在堆内存中。
+ 包含引用类型值的变量实际上包含的并不是对象本身，而是一个指向该对象的指针;
+ 从一个变量向另一个变量复制引用类型的值，复制的其实是指针。因此两个变量最终都指向同一个对象;
+ 确定一个值是那种基本类型可以使用typeof 操作符，确定一个值是那种引用类型可以用instanceof操作符。























