## 面向对象 ##

### 字面量创建数据和实例的方式创建数据 ###

1. 实例的创建方式，通过new操作符调用当前类型的构造函数，会得到当前类型的一个实例

- 创建引用数据类型：

```javascript
var ary2 = new Array(1, 2, 3, 4); // 用实例的方式创建一个数组
ary2.push(5);

var ary = [1, 2, 3, 4]; // 字面量创建数组
ary.push(5);
```
- 创建引用基本类型

```javascript

var str1 = new String('javascript');
console.log(str1.toUpperCase());

var str2 = 'javascript';
console.log(str2.toUpperCase());
console.log(typeof str1); // 'object'
console.log(typeof str2); // 'string'
```
	- 基本数据类型的只能通过字面量的方式创建，如果用实例的方式创建，这个实例将会是变成一个对象的基本数据类型
2. 字面量创建方式
- 创建引用数据类型：字面量创建的引用该数据类型和实例的方式创建的引用类型没有区别
- 创建基本数据类型

```javascript
var num = 1;
var str = 'abc';
var bool = true;
var empty = null;
var notDefined = undefined;
var sym = Symbol('cbd');
```

### new调用构造函数和普通调用的区别 ###

```javascript
function Teacher(name, age, subject, from) {
  this.name = name;
  this.mission = '传道受业解惑';
  this.age = age;
  this.subject = subject;
  this.from = from;
  this.teach = function () {
    console.log(`${name} 老师教 ${subject} 学科`);
  };
  // 上面这种通过this.xxx = xxx的方式直接向实例上添加的属性成为私有属性。
  var career = '前端'; // 这只是一个私有变量，不会和实例产生联系

}
// 普通函数执行：
let t1 = Teacher('马宾', 18, 'js', '珠峰');
console.log(t1); // undefined

let t2 = new Teacher('姜文', 19, 'js', '珠峰');
console.log(t2); // Teacher {name:......}

```

区别：
函数的普通调用过程：
1. 新开辟栈内存作为执行的作用域
2. 形参赋值
3. 私有作用域变量提升
4. 代码从上到下执行
5. 释放栈内存

new调用时：
1. 开辟栈内存
2. 形参赋值
3. 变量提升
4. 隐式创建一个实例对象，并且把当前构造函数中的this指向这个实例对象
5. 执行函数体中的代码，当遇到this.xxx = xxx时就是在向实例对象上增加属性
6. 隐式返回实例对象
7. 释放栈内存

### 原型对象 ###

原型prototype:每一个函数都有天生自带一个属性prototype(原型)，这个属性的值是一个对象，用来存储当前类型的共有属性和方法。保存在原型上的属性和方法称为公有的属性和方法

```javascript
function Teacher(name, age, subject, from) {
  this.name = name;
  this.mission = '传道受业解惑';
  this.age = age;
  this.subject = subject;
  this.from = from;
}

Teacher.prototype.teach = function () {
  console.log(`${this.name} 老师教 ${this.subject} 学科`);
};

console.log(Teacher.prototype);


let t1 = new Teacher('mabin', 18, 'js', 'zf');
let t2 = new Teacher('jiangwen', 19, '架构', 'zf');

t1.teach();
t2.teach();
console.log(t1.teach === t2.teach); // true
```

hasOwnProperty()方法：检测某个属性值是否事对象的私有属性，如果是则返回true

###原型链：对象属性查找机制###

每个实例都有一个属性proto属性，它指向当前实例所属类的prototype对象。当我们访问对象的一个属性时，如果有，就使用私有属性，如果没有就通过实例proto找到实例所属类的prototype上查找，如果找到就使用prototype上的属性，如果还没找到，就通过prototype的proto继续向上查找，一直到Object的prototype就停止查找，如果还没找到就返回undefined