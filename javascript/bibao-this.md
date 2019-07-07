## 闭包 this 面向对象基础 ##

###  闭包 ###

定义：函数执行时形成的一个私有作用域，保护里面的变量不受外界干扰，这种保护机制称为闭包

应用：
- 柯里化函数：把多参数的函数变成单参数的函数

	```javascript
	function fn(a){
		return function(b){
			return function(c){
				retutn a + b + c;
			}
		}
	}

	fn(1)(2)(3);
	```
- 利用闭包机制隔离全局命名空间

	```javasccript
	(function(){
		//自动执行函数也是闭包
		let a = 10; //a是私有变量，不会影响全局作用域
	})();
	```
- 惰性封装

	```javascript
	var utils = (function () {
	  var version = '1.0.1';
	  function sum(a, b) {
	    return a + b
	  }
	  function minus(a, b) {
	    return a - b;
	  }
	  return {
	    sum: sum,
	    minus: minus
	  }
	})();
	```
- 利用闭包的不销毁作用域保存数据：累加技术、选项卡闭包版本

## this ##
- 事件函数中的this是绑定该事件的元素
- 自执行函数中的this是window
- setTimeout/setInterval()定时器回调函数中的this指向window
- 方法调用时，看方法前面是否有点.如果有点前面是谁，this就是谁，如果没方法中的this就是window
- 箭头函数中的this指向函数定义时所在作用域中的this
- 全局作用域中的this是window
- this在运行时不可以赋值

### 箭头函数简化语法 ###
- 只有一个形参时，可以省略形参入口的小括号
- 如果函数只有一行代码，或者只有return指定返回值，可以省略函数体的花括号和return关键字


## 面向对象 ##

定义：是一种对现实世界的理解和抽象的方法
研究范畴
- 对象：万事万物都是对象，每个对象都具备各自的属性和方法
- 类：抽象事物的特性和特点，把对象分成不同的类型。描述一群事物的共同特点的抽象概念
- 实例：类中的一个具体的个体。具有这个类所有的特性和功能

#### JS中的内置类 ####
- Object
- Array
- Date
- RegExp
- Function

这些内置类都是函数数据类型的

instanceof运算符：检测对象是否是某个类型的实例，如果是返回true