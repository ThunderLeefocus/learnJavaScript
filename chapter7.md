### 第七章 函数表达式

本章内容：函数表达式的特征、使用函数实现递归、使用闭包定义私有变量

#### 7.1 递归

arguments.callee() 是指向正在执行的函数的指针，可以用来实现递归调用。


#### 7.2闭包

##### 7.2.1 闭包与变量

##### 7.2.2 关于this对象

匿名函数的执行环境具有全局性，因此this 对象通常指向window。当函数被作为某个对象的方法调用时，this等于那个对象。

##### 7.2.3 内存泄露

#### 7.3 模仿块级作用域

JavaScript中没有块级作用域的概念。

JavaScript 从来不会告诉你是否多次声明了同一个变量；遇到这种情况，只会对后续声明视而不见。

匿名函数可以用来模仿块级作用域避免这个问题。

​	块级作用域（私有作用域）的匿名函数：

(function(）{

​	// 这里是块级作用域

}）();

以上，定义并立即调用了一个匿名函数。将函数声明包含在圆括号中，表示实际上是一个函数表达式，紧随其后的另一对圆括号会立即调用这个函数。

```
var someFunction = function(){
	//这里是块级作用域
};

someFunction();
```

上述例子，先定义函数，然后立即调用它。定义函数的方式是创建一个匿名函数，并把匿名函数赋值给someFunction().调用函数的方式是在函数名称后面加一对圆括号。someFunction()。

这种技术经常在全局作用域中被用在函数外部，从而限制向全局作用域添加过多的变量和函数。一般来说尽量少向全局作用域添加变量和函数。过多全局变量和函数容易导致命名冲突。通过创建私有作用域，每个开发人员既可以使用自己的变量，也不必担心搞乱全局作用域。

#### 7.4 私有变量

严格来将，JavaScript中没有私有成员概念；所有对象属性都是公有的。倒是有私有变量的概念。任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数外部访问这些变量。私有变量包括：函数的参数、局部变量和在函数内部定义的其他函数。

```
function add(num1, num2){
	var sum = num1 + num2;
	return sum;
}
// 函数内部有3个私有变量：num1，num2，sum
// 我们把有权访问私有变量和私有函数的公有方法称为特权方法（Privilege method）


//eg2
function MyObject(){
	//私有变量和私有函数
	var privateVariable = 10;
	
	function privateFunction(){
		return false;
	}
	
	// 特权方法
	this.publicMethod = Function (){
		privateVariable++;
		return privateFunction();
	};
}
// 对这个例子，变量privateVariable和函数privateFunction()只能通过特权方法publicMethod()来访问。创建MyObject的实例之后，除了使用publicMethod()这一个途径以外，没有任何办法能够直接访问privateVariable 和 privateFunction().

```

利用私有和特权成员，可以隐藏不该直接被修改的数据：

```
function Person(name){
	this.getName = function(){
	return name;
	};
	this.setName = function (value){
		name = value;
	};
}
var person = new Person("Nicholas");
alert(person.getName()); //"Nicholas"
person.setName("Greg");
alert(person.getName()); //"Greg"

```

构造函数中定义了两个特权方法：getName 和 setName.这两个方法都可以在构造函数外部使用，都有权访问私有变量name。但是在Person构造函数外部，没有任何办法能够访问name.

私有变量name 在Person的每一个实例中都不相同，因为每次调用构造函数都会重新创建这两个方法。

##### 7.4.1 静态私有变量

通过在私有作用域中定义私有变量或者函数，也可以创建特权方法

```
(function (){
	//私有变量和私有函数
	var privateVariable = 10;
	
	function privateFunction(){
		return false;
	}
	
	//构造函数
	MyObject = function(){
	};
	
	//公有/特权方法
	MyObject.prototype.publicMethod = function(){
		privateVariable++;
		return privateFunction();
	};
})();
```

这个模式创建了一个私有作用域，并在其中封装了一个构造函数和相应方法。

注意：这个模式在定义构造函数时，并没有使用函数声明，而使用函数表达式。函数声明智能创建局部函数。那不是我们想要的。处于同样原因，我们也没有在声明MyObject时使用`var`关键字.

记住：初始化未经声明的变量，总是会创建一个全局变量。因此MyObject就成了全局变量，能在私有作用域之外被访问。严格模式下给未经声明的变量赋值会导致错误。



##### 7.4.2 模块模式

模块模式（module pattern) 是为单例创建私有变量和特权方法。所谓单例，指的是只有一个实例的对象。

按照惯例，JavaScript是以对象字面量来创建单例对象：

```
var singleton = {
	name : value,
	method:function(){
		//这里是方法代码
	}
}；
```

模块模式，通过为单例添加私有变量和特权方法使其得到增强，其语法形式如下：

```
var singleton = function(){
	
	//私有变量和私有函数
	var privateVariable = 10;
	function privateFunction(){
		return false;
	}
	//特权/公有方法和属性
	return{
		publicProperty: true,
		
		publicMethod:function(){
			privateVariable++;
			return privateFunction();
		}
	};
}();
```

##### 7.4.3 增强的模块模式

```
var singleton = function(){

	//私有变量和私有函数
	var privateFunction(){
		return false;
	}
	
	//创建对象
	var object = new CunstomType();
	
	//添加特权/公有属性和方法
	object.publicProperty = true;
	
	object.publicMethod = function(){
		privateVariable++;
		return  privateFunction();
	};
	
	//返回这个对象
	return object;
}();
```



#### 7.5 小结

