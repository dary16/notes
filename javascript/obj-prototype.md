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

### JS中的面向对象 ###

面向对象的研究范畴：类、封装、继承和多态

1. 类：js中的类都是一个函数数据类型，都天生自带一个prototype属性，它的值是一个对象
2. prototype:每个prototype对象都天生自带一个属性constructor,这个属性的值指向当前类的构造函数本身
3. 对象都有一个proto属性，这个属性指向当前实例所属类的prototype

#### 内置类：Array、String、Number、Function、Date等 ####

### 原型上添加共有属性的方法 ###
- 直接给原型添加方法

```javascript
Fn.prototype.say = 'hello';
Fn.prototype.title = 'world';
Fn.prototype.greeting = function () {
  console.log('hi~');
};
```
- 通过实例对象的proto,因为实例的proto指向当前实例所属类的原型对象，所以可以通过修改实例的proto来修改原型对象

```javascript
let f1 = new Fn();
 f1.__proto__.say = 'hello';
 f1.__proto__.hello = 'world';
 f1.__proto__.greeting = function () {
  console.log('hi~')
 };
```
- 修改原型对象的指向

```javascript
Fn.prototype = {
  say: 'hell',
  hello: 'world',
  greeting: function () {
    console.log('hi~')
  }
};
console.log(Fn.constructor); // Object
Fn.prototype.constructor = Fn;
// 当需要批量给原型增加属性或者方法时，我们需要把一个新的对象赋值给类的原型时，此时要给这个对象增加一个constructor属性
```

#### 选项卡插件封装 ####

```javascript
// 1. 创建选项卡类
function Tab(options) {
  // 1. 确保如何是通过new操作调用
  if (!(this instanceof Tab)) {
    console.error('Tab is a constructor which should be call with new');
    return;
  }

  // 2. 参数合法校验
  if (!options || !options.el) {
    console.error('缺少el元素');
    return;
  }

  // 3. 将传进来的参数对象保存到实例上
  this._options = options;

  // 4. 执行初始化
  this.init();
}



// 为tab增加init方法：
Tab.prototype.init = function () {
  this.queryEle();
  this.bindEvent();
};

// 为Tab增加公用的获取元素的方法
Tab.prototype.queryEle = function () {
  // 1. 从this中的options中的el获取最外层元素
  const container = document.querySelector(this._options.el);

  // 2. 获取选项卡头，并挂载到实例上
  this.headerList = container.querySelectorAll('.header > li');

  // 3. 获取所有的卡片并挂载到实例上
  this.cardList = container.querySelectorAll('div');
};

// 为Tab增加公用的绑定事件的元素
Tab.prototype.bindEvent = function () {
  const HEADER_LIST = this.headerList; // 用一个常量缓存headerList
  // 变量headerList给每个li绑定点击事件
  for (let i = 0; i < HEADER_LIST.length; i++) {
    HEADER_LIST[i].onclick = () => {
      // 这里使用箭头函数，是因为我们希望这里的this是Tab的实例，如果不使用箭头函数，点击事件函数中的this就是选项卡头了
      this.clearClass();
      this.addClass(i);
    }
  }
};

// 为Tab类增加移除类名的方法
Tab.prototype.clearClass = function () {
  const HEADER_LIST = this.headerList;
  const CARD_LIST = this.cardList;
  for (let i = 0; i < HEADER_LIST.length; i++) {
    HEADER_LIST[i].className = '';
    CARD_LIST[i].className = '';
  }
};

// 为Tab类添加类名的方法
Tab.prototype.addClass = function (index) {
  this.headerList[index].className = 'active';
  this.cardList[index].className = 'active';
};

new Tab({
  el: '#tab1'
});

new Tab({
  el: '#tab2'
});
```


