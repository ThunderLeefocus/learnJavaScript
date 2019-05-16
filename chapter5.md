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

```
var colors = ["red", "blue", "green"]; // 创建一个包含3个字符串的数组
colors[99] = "black";
alert(colors.length); //100
```

##### 5.2.1 检测数组

Array.isArray() 方法的目的是追中确定某个值到底是不是数组，而不管它在哪个全局执行环境中创建。



##### 5.2.2 转换方法

所有对象都有toLocaleString（）、toString（）、valueOf()方法。调用valueOf 返回数组本身。调用toString 返回由数组每个值的字符串形式拼接成的用逗号分隔的字符串。toLocaleString（）经常返回与上述两个相同的值，它会创建一个数组值的以逗号分隔的字符串。

join（）方法重现了toString输出，给join（）传递什么分隔符，结果就是用什么分隔符分隔的数组值。



##### 5.2.3 栈方法

数组表现的像栈一样。LIFO(Last In First Out).栈中的插入（推入），移除（弹出）只发生在栈顶。

ECMAScript 为数组专门提供了push() 和 pop()方法。

push()可以接受任意数量的参数，把它们诸葛添加到数组末尾，并返回修改后数组的长度。pop()方法则从数组末尾移除最后一项，减少数组长度，然后返回移除的项。



##### 5.2.4队列方法

队列在列表末端添加项，在列表前段移除项。访问规则FIFO(First in First Out).

模拟队列从数组前端取得项的方法是shift(),能够移除数组中的第一个项，并返回该项，同时数组长度减1.

```
var colors = new Array();
var count = colors.push("red", "green"); // 推入两项
alert(count); //2

count = colors.push("black");
alert(count); //3

var item = colors.shift(); //取得第一项
alert（item); // “red"
alert(colors.length); //2 
```



ECMAScript 还为数组提供了unshift()方法。与shift（）用途相反；在数组前端添加人一个项并返回新的数组长度。用unshift()和pop()方法，可以从相反方向模拟队列。



##### 5.2.5重排序方法

数组中存在的两个可以直接重排序的方法，reverse（）、sort（）。

reverse()方法会反转数组项的顺序。

sort()会按升序排列数组项，sort（）会调用每个数组项的toString()转型方法，比较得到的字符串，即使数组中每一项都是数值，sort（）方法比较的也是字符串.

```
var values = [0, 1, 5, 10, 15];
values.sort();
alert(values); //0,1,10,15,5
```

数值5 虽然小于10，但是字符串比较时，“10”则位于“5”前面。

要给sort（）方法传递一个比较函数。

```
function compare(value1, value2){
	if (value1 < value2){
		return -1;
	}else if (value1 > value2){
		return 1;
	}else{
		return 0;
	}
}

var values = [0, 1, 5, 10,15];\
values.sort(compare);
alert(values);//0,1,5,10,15

```

W3C 规范：

**arrayObject.sot(sortby)**

| 参数   | 描述                           |
| ------ | ------------------------------ |
| sortby | 可选。规定排序顺序。必须是函数 |

##### 5.2.6 操作方法

1.concat()方法基于当前数组中的所有项创建新的数组。接收到的参数添加到新的副本的末尾，返回新构建的数组。



2.slice() ,基于当前数组中的一或多项创建一个新数组。可以接受1或两个参数、及要返回项的起始和结束为止。只有一个参数，返回从指定位置到数组末尾所有项。两个参数，返回起始和结束位置之间的——不包括结束位置（左闭右开）。

并且slice（）不会影响原始数组。

3.splice（）方法：主要用途是向数组的中部插入项，用法有3种：

1. 删除：删除任意数量的项；制定2个参数：要删除的第一项位置和要删除的项数。例如splice（0,2）就是删除数组前两项；
2. 插入：可以向指定位置插入任意数量项目，提供3个参数：起始位置、0（要删除的项数）和要插入的项数。如果要插入多项，可以再传入第四、第五任意多项目。
3. 替换：可以向指定位置插入任意数量项，同时删除任意数量项，指定3个参数：起始位置、要删除的项数和要插入的项数。插入的项数不必与删除的相等。

```
var colors = ["red", "green", "blue"];
var removed = colors.splice(0,1);//删除第一项
alert(colors);// green,blue
alert(removed);//red,返回的数组只包含一项

removed = colors.splice(1, 0, "yellow", "orange")； //从第位置1开始插入两项
alert(colors); //green,yellow,orange,blue
alert(removed); //返回的是一个空数组

removed = 。splice(1, 1, "red", "purple");
alert(colors); // green,red,purple,orange,blue
alert(removed); //yellow，返回数组包含一项
```

##### 5.2.7 位置方法

为数组实例添加两个位置方法：indexOf()和lastIndexOf().都接收2个参数，要查找的项和（可选）表示查找起点位置的索引。其中indexOf（）方法从数组的开头开始向后查找，lastIndexOf（）方法从数组的末尾开始向前查找。

返回要查找的项在数组中的位置，或者在没找到的情况下返回-1.查找的项必须严格相等（就像使用===）.



##### 5.2.8 迭代方法

ECMAScript5为数组定义了5个迭代方法。每个方法接收两个参数：在每一项上运行的函数和（可选）运行该函数的作用域对象。

传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身。

+ every()：对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true。
+ filter(): 对数组每一项运行给定函数，返回该函数会返回true的项组成的数组；
+ forEach(): 对数组每一项运行给定函数，这个方法没有返回值。
+ map（）： 对数组每一项运行给定函数，返回每次函数调用的结果组成的数组。
+ some(): 对数组每一项运行给定函数，如果该函数对任一项返回true，则返回true。

##### 5.2.9 归并方法

reduce（） 和 reduceRight() .两个方法都迭代所有项，构建一个最终返回值。reduce（）从第一项开始，reduceRight（）从最后一项开始，向前遍历。

接收两个参数：一个在每一项上调用的函数 和（可选）最为归并基础的初始值。

传给归并函数的函数接收4个参数：前一个值、当前值，项的索引和数组对象。第一次迭代发生在数组第二项上，因此第一个参数是数组第一项，第二个参数是数组第二项。



#### 5.3 Date类型

##### 5.3.1 继承的方法

##### 5.3.2 日期格式化方法

##### 5.3.3 日期、时间组件方法

P102



#### 5.4 RegExp类型

var expression =  / pattern / flags ;

pattern 部分可以使任何正则表达式。每个正则表达式可以带1个或者多个标志（flags）,用于标明正则表达式的行为。

+ g:表示全局（global）模式，即模式将被应用于所有字符串。
+ i： 表示不区分大小写(case-insensitive)模式，在确定匹配项时忽略模式与字符串的大小写；
+ m：表示多行（multiline）模式，即在到达一行文本末尾时还会继续查找下一行是否存在模式匹配项。

除了使用字面量定义，还可以用构造函数来定义

```
/*
* 匹配第一个“bat”或“cat”，不区分大小写
*/
var pattern1 = /[bc]at/i;

/*
* 与pattern1 相同，只不过是使用构造函数
*/
var pattern2 = new RegExp("[bc]at", "i");
```



##### 5.4.1 RegExp 实例属性

##### 5.4.2 RegExp实例方法

主要方法exec().方法接收一个参数，即要应用模式的字符串，然后返回包含第一个匹配项信息的数组。没有匹配项的情况下返回null。返回的数组时Array的实例，包含两个额外的属性：index 和 input 。其中，index 表示匹配项在字符串的位置，而input表示应用正则表达式的字符串。

第二个方法,test（），它接收一个字符串参数。在模式与该参数匹配的情况下返回true；否则返回false.

```
var text = "000-00-0000";
var pattern = /\d{3}-\d{2}-\d{4}/;

if (pattern.test(text)){
	alert("The pattern was matched.")
}
```

##### 5.4.3 RegExp构造函数属性

##### 5.4.4 模式的局限性

#### 5.5 Function类型

每个函数都是Function类型的实例，而且都与其他引用类型一样具有属性和方法。由于函数是对象。因此函数名实际上也是指向函数对象的指针，不会与某个函数绑定。

##### 5.5.1 没有重载（深入理解）

##### 5.5.2 函数声明与函数表达式

解析器在向执行环境中加载数据时，会率先读取函数声明，并使其在执行任何代码之前可用。而函数表达式，必须等到解析器执行到它所在代码行才会真正被解释执行；

```
alert(sum(10, 10));
function sum(num1, num2){
	return num1 +num2;
}//代码可以正常运行


alert(sum(10, 10));
var sum = function(num1, num2){
	return num1 +num2;
};  //代码运行错误
```

由于函数位于一个初始化语句中，而不是一个函数声明。

##### 5.5.3 作为值的函数

不进可以像传递参数一样把一个函数传递给另一个函数，而且可以将一个函数作为另一个函数的结果返回。



##### 5.5.4 函数内部属性

函数内部，两个特殊对象：arguments 和 this。

arguments 主要用途，保存函数参数，这个对象还有一个叫callee的属性，该属性是一个指针，指向拥有arguments对象的函数。

例子：阶乘函数

```
function factorial(num){
	if(num <= 1){
		return 1;
	}else{
		return num *factorial(num -1);
	}
}
```

这样的递归算法没有问题，但是这个函数的执行与函数名factorial紧紧耦合在了一起。为了消除这种紧密耦合现象。可以使用arguments.callee.

```
function factorial(num){
	if(num <= 1){
		return 1;
	}else{
		return num * arguments.callee(num - 1);
	}
}

var trueFactorial = factorial;
factorial = function(){
	return 0;
};
alert(factorial(5)); //0
alert（trueFactorial(5));//120
```

变量trueFactorial获得了factorial的值，实际上是另外一个位置保存了指向函数的指针。这样即使factorial被更改，trueFactorial依然可以计算阶乘。

​	函数内部另一个特殊对象this，this引用的是函数执行的环境对象。

##### 5.5.5 函数的属性和方法

函数有属性和方法。每个函数都包含两个属性：length 和 prototype。

#### 5.6 基本包装类型

##### 5.6.1 Boolean类型

建议不用

##### 5.6.2 Number类型

一般不建议使用

##### 5.6.3 String类型

1. 字符方法
2. 字符串操作方法
3. 字符串位置方法
4. trim()方法：创建一个字符串的副本， 删除前置及后缀的所有空格，然后返回结果。
5. 字符串大小写转换方法。
6. 字符串的模式匹配方法。
7. localeCompare（）方法











