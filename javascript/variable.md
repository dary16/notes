## 变量提升 ##

- 在js代码执行之前，对所有声明的变量进行提前，带var和带function变量进行提前声明和定义；
	- 声明：声明一个变量，告诉浏览器有一个变量
	- 定义：给变量赋值
- 对于带var的进行提前声明，不赋值
- 对于带function的进行提前声明，并且赋值为函数定义本身，变量提升结束后，代码才会从上到下执行
- 当执行到var 变量=值；时，此时才会对变量进行赋值，即在此之后变量才是代表这个值本身，在此之前是undefined
- 当执行到function 函数名 时，跳过，因为在变量提升阶段就已经完成了对变量的赋值过程



```javascript
console.log(num);  //undefined

var num = 100;
console.log(num); //100
console.log(fe);

fe(); //123
function fe(){
	console.log(123);
}
```

## js的运行机制及原理 ##

### js运行的环境（栈内存）：作用域 ###

作用域是js运行的环境，另外一个功能是保存基本数据类型，在Js中作用域分为：
- 全局作用域：当页面打开时，首先形成一个全局作用域，执行全局中的代码，全局作用域是window
- 私有作用域（函数作用域）：当执行一个函数作用域，这个作用域用来保存函数中的基本数据类型，同时执行函数代码
- 块级作用域（类似私有作用域ES6）


### js运行过程 ###

- 在js代码执行前，浏览器会开辟一个全局作用域，然后执行变量提升，完成标量提升操作后开始执行
- 当执行时，如果遇到基本数据类型，就在作用域中存储该基本数据类型
- 如果遇到引用数据类型，则浏览器会再次分配一个堆内存，然后把引用数据类型的内容存储到堆内存中，再把这个堆内存的地址赋值给变量
- 遇到函数执行时会经历以下几步：
	- 浏览器开辟一个私有作用域
	- 形参赋值，把执行时的实参赋值给函数形参变量
	- 私有作用域中变量提升
	- 函数代码从上到下执行


### 私有变量和全局变量 ###

- 全局变量：在全局作用域中声明的变量
- 私有变量：函数的形参以及在函数私有作用域中声明的变量

## 重复声明的问题 ##

- 同名变量只会声明一次，代表的值就是最后一次的值
- 变量提升时，function的优先级高于普通变量

```javascript
console.log(fe); //函数体  12

function fe(){
	console.log(12);
}

var fe = 123; //当执行到这里，变量值为123
```
- 如果变量名和函数名同名，在执行到变量的赋值语句之前时，这个名字代表函数，但是执行过后，代表赋值语句

```javascript
function fe(){
	console.log('fe);
}

var fe = 1;

fe(); //报错，因为执行到这里的时候fe不再代表一个函数了
```

### 变量提升的细节问题 ###

- 等号右侧的不会进行变量提升，即使右侧是函数也不会进行变量提升
- 条件语句中的变量不管条件成立与否都会参与当前作用域中的变量提升
- 函数中，return下面的代码虽然不执行，但仍会进行函数作用域中的变量提升
- 函数的返回值不参与变量提升，return右边的不会参与变量提升

```javascript
function aa(a,b){
	console.log(foo); //报错，foo没有定义
	return function foo(){
		console.log('函数的返回值不参与变量提升');
	}
}
aa();
```

### 带var和不带var的区别 ###

- 在全局中，带var和function声明的变量，相当于给window上添加量一个同名属性
- 不带var的不会参与变量提升

## 作用域链 ##

js中的作用域
- 全局作用域
- 私有作用域
- 块级作用域

变量的查找机制：
当在作用域中查找一个变量的时候，先看当前作用域中是否声明过这个变量，如果声明过，就使用这个变量，如果没有声明过，那么就去上级作用域查找，找到就使用，如果没有就继续往上查找，直到找到winodw为止。




    