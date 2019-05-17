### 第六章 面向对象的程序设计

本章内容：理解对象属性、理解并创建对象、理解继承

#### 6.1 理解对象

​	创建自定义对象最简单方式，创建一个Object的实例，然后为它添加属性和方法：

```
var person = new Object();
person.name = "Nicholas";
person.age = 29;
person.job = "Software Engineer";

person.sayName = function(){
	alert(this.name);
};
```

```
//面向字面量创建对象成为首选模式
var person = {
	name: "Nicholas",
	age: 29,
	job: "Software Engineer",
	
	sayName: function(){
		alert(this.name);
	}
};
```

##### 6.1.1属性类型

1. 数据属性

   包含一个数据的位置，在这个位置可以读取和写入值。有四个描述其行为的特性：

   + [[configurable]]  
   + [[Enumerable]]
   + [[Writable]]
   + [[Value]] 

要修改属性默认的特性，必须使用ECMAScript5的Object.defineProperty()方法。

2. 访问器属性：不包含数值，包含一对儿getter 和 setter 函数。getter负责返回有效的值，setter负责决定如何处理数据、访问器属性有4个特性：
   - [[configurable]]  
   - [[Enumerable]]
   - [[Get]]
   - [[Set]] 

##### 6.1.2 定义多个属性

##### 6.1.3读取属性的特性

#### 6.2 创建对象

##### 6.2.1 工厂模式

开发人员发明一种函数，用函数来封装以特定接口创建对象的细节。

```
function createPerson(name, age, job){
	var o = new Object();
	o.name = name;
	o.age = age;
	o.job = job;
	o.sayName = function(){
		alert(this.name);
	};
	return o;
}

var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
```

##### 6.2.2 构造函数模式

```
function Person(name, age, job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = function(){
		alert(this.name);
	};
}

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
```



**构造函数**与工厂模式的不同：

+ 没有显式地创建对象；

+ 直接将属性和方法赋给this对象；

+ 没有return语句。

  函数名Person使用大写P开头，构造函数始终要以一个大写字母开头。

  创建Person的新实例，必须使用new 操作符：

  1. 创建一个新对象；
  2. 将构造函数的作用域赋给新对象（因此this就指向了这个新对象）；
  3. 执行构造函数中的代码（为新对象添加属性）；
  4. 返回新对象。

   

1）将构造函数当做函数

​	

2）构造函数的问题

##### 6.2.3 原型模式

​	我们创建的每个函数都有一个prototype(原型)属性，属性是一个指针，指向一个对象，这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。即prototype是通过调用构造函数而创建的那个对象实例的原型对象。好处是可以让所有对象实例共享它所包含的属性和方法。

```
function Person(){
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
	alert(this.name);
};

var person1 = new Person();
person1.sayName(); //"Nicholas"

var person2 = new Person();
Person2.sayName(); //"Nicholas"
alert(person1.sayName == person2.sayName); //true
```

*1.理解原型对象*

当代码读取某个对象的某个属性时，都会执行一次搜索，目标是具有给定名字的属性。搜索首先从对象实例本身开始。如果实例中找到了具有给定名字的属性，则返回该属性值；如果没有找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性。在原型对象中找到了这个属性，就返回属性的值。

可以通过对象实例范文保存在原型中的值，但是不能通过对象实例重写原型中的值。如果在实例中添加了一个属性，改属性与实例原型中的属性同名，那么在实例中创建该属性，该属性会屏蔽原型中的属性。

```
function Person(){
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
	alert(this.name);
};
var person1 = new Person();
var person2 = new Person();

person1.name = "Greg";
alert(person1.name);//"Greg"--来自实例
alert(person2.name);//"Nicholas"--来自原型
```

为对象实例添加一个属性，添加这个属性只会阻止我们访问原型中的那个属性，但是不会修改那个属性。使用delete操作符，可以完全删除实例属性，从而我们能够冲访问原型中的属性。

使用hasOwnProperty()方法可以检测一个属性是存在于实例还是原型中。

*2.原型与 in 操作符* 

​	两种方式使用 `in` 操作符:单独使用 和 在`for-in`循环中使用

`in`操作符会在通过对象能够访问给定属性时返回true，无论该属性存在于实例还是原型中。

`for-in` 循环时，返回的是所有能够通过对象访问的、可枚举的属性。实例中、原型中的都包括。

*3.更简单的原型语法*

```
funciton Person(){
}
Person.prototype = {
	name:"Nicholas",
	age: 29,
	job: "Software Engineer",
	sayName: function(){
		alert(this.name);
	}
};
```

*4.原型的动态性*

*5. 原声对象的原型*

所有原声引用类型（Object、Array、String，等）都在其构造函数的原型上定义了方法。

通过原声对象的原型，也可以定义新方法，一般不推荐在产品化程序中修改原声对象原型，有可能会在另一个支持该方法的实现中导致命名冲突。

*6. 原型对象的问题*

原型模式最大的问题由其共享的本质导致。因为原型中所有属性被很多实例共享，然而对于包含引用类型值的属性来说，问题出现了:

```
function Person(){
}
Person.prototype = {
	constructor: Person,
	name:"Nicholas",
	age: 29,
	job: "Software Engineer",
	friends: ["Shelby", "Court"],
	sayName: function(){
		alert(this.name);
	}
};

var person1 = new Person();
var person2 = new Person();

person1.friends.push("van");

alert(person1.friends);//"Shelby, Court, Van"
alert(person2.friends);//"Shelby, Court, Van"
alert(person1.friends === person2.friends ); //true
```

##### 6.2.4 组合使用构造函数模式和原型模式

​	创建爱你自定义类型最常见方式，组合使用构造函数模式与原型模式。构造函数模式用于定义实例属性，原型模式用于定义方法和共享的属性。每个实例都会有自己的自己的实力属性的副本，同时又共享者对方法的引用，最大限度节省内存。混成模式还支持向构造函数传递参数；

```
function Person(name, age, job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.friends = ["Shelby", "Court"]；
}
Person.prototype = {
	constructor : Person,
	sayName : function(){
		alert(this.name);
	}
}

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
person1.friends.push("Van");
alert(person1.freiends);//"Shelby,Court,Van"
alert(person2.friends);//"Shelby,Court"
alert(person1.sayName === person2.sayName);//true
```

​	这种构造与原型混成模式，是目前使用最广，认同度最高的方法。

##### 6.2.5 动态原型模式

```
function Person(name, age, job) {
    //属性
    this.name = name;
    this.age = age;
    this.job = job;
    
    //方法
    if (typeof this.sayName != "function"){
        
        Person.prototype.sayName = function () {
            alert(this.name);
            
        };
    }
}
var friend = new Person("Nicholas", 29, "Softeware Engineer");
friend.sayName();
```

​	使用动态原型模式时，不能使用对象字面量重写原型。否则会切段现有实例与新原型的联系。



##### 6.2.6 寄生构造函数模式

