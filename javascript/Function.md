## function、call、apply、bind ##

### 函数 ###

- 所有的函数都是Function的一个实例
- 内置类和Function类
	- 内置类都是Function类的实例，而实例都有一个__proto__指向所属类的原型对象。js中所有的函数都是Function的实例

	```javascript
	Array.__proto__ === Function.prototype;  true

	console.log(Date instanceof Function);  true
	console.log(Date.__proto__ === Function.prototype);  true
	console.log(RegExp instanceof Function);  true
	console.log(RegExp.__proto__ === Function.prototype);  true
	console.log(Object instanceof Function);  true
	console.log(Object.__proto__ === Function.prototype);  true
	
	console.log(Function.prototype.__proto__ === Object.prototype);  true
	```
- 因为Object也是一个类，所以也是一个函数，所以也是Function的实例，Object.proto指向Function.prototype
- Function本身也是一个类，也是函数，所以Function也有prototype,而prototype是一个对象，Function.prototype.__proto__又指向了Object.prototype

#### Function和Object的关系 ####

1. Object.__proto__指向Function.prototype
2. Function.prototype.__proto__指向Object.prototype
3. 所有的函数都是Function的实例
4. 所有的引用数据类型都是Object这个基类的实例，所以函数也是对象

```javascript
console.log(Function instanceof Object); //true
console.log(Array instanceof Object); //true
console.log(Date instanceof Object); //true
let obj = {
  id: 1
};
console.log(obj instanceof Object); //true

function fn() {
  console.log('fn') //10
}
fn.name = '珠峰';
fn.age = 10;
console.log(fn.age); //10
```

#### 总结 ####
- 所有的函数数据类型都是Function的实例
- Function函数类本身也是一个函数
- 因为Function是自己的实例，所以Function.proto指向自己的prototype
- Function也是Object基类的实例，所以函数也是对象，可以有自己的私有属性
- js内置引用类型都是Function的实例

### 函数的三种角色 ###

1. 作为一个普通函数执行（形参、实参、返回值）
2. 作为一个类（new Fn 构造函数执行）
3. 函数也是一个普通对象

#### 1.普通函数 ####

```javascript
function sum(a,b){
  var x = 1;
  var y = 12;
  var z = 123;
  return a + b + x + y + z;
}
var result = sum (1,3);
console.log(result); //140
```

##### 普通函数执行过程 #####

1. 开辟一个新的作用域
2. 形参赋值
3. 变量提升
4. 函数体从上到下执行
5. 销毁作用域

#### 构造函数（类） ####

- 每个构造函数都有一个prototype属性，它的值是一个对象，用来存放当前类型的共有属性和方法
- 必须通过new操作符调用函数才能返回一个实例对象
- 在原型上增加的方法都是这个类共有的属性和方法


##### new执行构造函数 #####

1. 新开辟一个作用域
2. 形参赋值
3. 变量提升
4. 隐式创建一个当前类的实例对象，并且把构造函数中的this指向当前实例
5. 执行构造函数中的代码
6. 隐式返回实例对象，相当于return this
7. 销毁作用域

#### 作为一个普通对象 ####

- 把函数当普通对象使用，就像操作普通对象一样操作对象，通过这样的方式添加给函数的属性或者方法称为静态属性或方法
- 通过 函数名.xxx = xxx添加的属性都是这个函数的私有属性。如果这个函数被当做构造函数使用时，这些属性既不是实例的属性也不是实例的公有属性，只能通过 函数名.xxx的方式获取

##### Array.isArray:是数组的静态方法，只能通过Array自己调用 #####


### call、apply、bind ###

this:是js代码执行时的环境对象，一般在函数中使用，在函数执行时，根据函数的调用方式不同而不同，在运行时不能通过赋值的方式修改

#### this常见的情况 ####
1. 事件中的this是绑定当前事件的远胜于
2. 自执行函数的this指向window
3. 定时器回调函数的this指向window
4. 全局作用域的this指向window
5. 方法调用时看方法前有没有.，如果有点前面是谁this就指向谁，没有this就指向window
6. 箭头函数中的this是箭头函数声明时所在作用域中的this
7. 构造函数中的this指向当前实例

Function.prototype上的三个方法call、apply、bind来修改函数中this的指向

#### call ####
- 语法：函数名.call(ctx,实参1，实参2,...)
- 参数：ctx将函数中的this修改为ctx;从第二个参数开始，后面的参数都是传递给函数执行的实参
- 作用：修改方法中的关键字为call方法的第一个实参ctx,并且把后面的参数当做实参传给函数，最后让函数执行

```javascript
function fe(a, b) {
  console.log(a, b);
  console.log(this);
}

fe(1, 2); // this -> window

var obj = {
  id: '0511120117'
};
fe.call(obj, 2, 5); //this指向Obj


```

#### apply ####
- 语法：函数名.apply（ctx,[实参1，实参2，...]）
- 参数：ctx将函数中的this修改为ctx,第二个参数是一个数组，数组项都是传递给函数的实参
- 作用：修改函数中的this关键字，并且接收一个由实参组成的数组，最后把这个数组作为实参传递给函数，让函数执行
- call和apply的区别：传递实参的方式不同



#### bind修改函数中的this关键字 ####

- 语法： 函数名.bind(ctx,实参1，实参2，...)
- 作用：绑定函数中的this关键字，并且返回一个绑定this的新函数；注意：bind不会让函数执行