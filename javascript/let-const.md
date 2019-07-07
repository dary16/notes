# JS引擎中的内存分类 #
- 栈内存：供JS代码执行的环境，保存基本数据类型
- 堆内存：存储引用数据类型

## 堆内存的创建和销毁 ##

创建：创建一个对象、数组、函数等引用数据类型，浏览器都会分配一块堆内存地址，存储引用数据类型的数据

```javascript
var obj = {
	name:'张三',
	age:10
}; //浏览器开辟一个堆内存，并把这个堆内存地址给obj变量

var obj2 = obj; //此时obj2也引用了这一块堆内存
```

堆内存释放：让所有的空间地址变量赋值为Null即可

```javascript
obj = null;
obj2 = null;
```

## 栈内存开辟和释放 ##

创建：
- 当浏览器打开时，首先会开辟形成一个顶层的栈内存，就是全局作用域
- 函数执行时，也会开辟一个供函数代码执行的栈内存（私有作用域）

函数每次执行，都会形成一个全新的栈内存，即每次函数执行都是在一个全新的环境里面执行，所以函数每次执行都是互相独立的

销毁：
- 全局栈内存：当页面关闭时才会销毁
- 函数的私有作用域：一般函数执行完成后，栈内存自动销毁
- 栈内存不被销毁的：函数执行完以后，当前形成的栈内存中，某些内容被栈内存以外的变量占用了，此时栈内存不能释放，栈内存不销毁，保存在栈内存中的数据也不会被销毁


# let & const #

## let const是ES6新增的关键字，用于声明变量 ##
- let声明普通变量
- const用于声明常量

## let const var的区别 ##

- let,const不存在变量提升
- let,const不能重复声明变量
- let,const在全局声明的变量或者常量不会像window上增加属性
- let,const在代码块中会出现，形成块级作用域，并且出现暂时性死区
	- if(condition){条件的花括号中使用let const就会形成块级作用域}

	```javascript
	let num1 = 2;
	if(true){
	let num1 = 4;
	console.log(num1); //4
	}
	```
	- for(...){for循环中的代码块}

	```javascript
	for(let j = 0;j < 3;j++){
	console.log('j不会泄露出去');
	}
	console.log(j); //not defined
	```
	- 利用{}形成的块级作用域

暂时性死区：在代码块中，用let const声明的变量，不能在声明之前使用

### const声明变量的细节问题 ###
- const声明时必须赋值
- 值一旦被定义，不能被修改
- 如果const声明的常量是一个引用数据类型，那么常量代表的引用地址不能该表，但是堆内的内容是可以修改的