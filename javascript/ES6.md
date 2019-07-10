# 深入理解ES6总结 #

## 块级绑定 ##

块级声明指变量在指定的作用域外无法访问，块级作用域在如下情况下被创建：
- 在一个函数内部
- 在一个代码块（由一对花括号包裹）内部

let声明会将作用域限制在当前代码块中，不会声明提前

const声明常量，并且要在声明时进行初始化，值被设置后不能再改变

const声明对象，不会阻止对变量成员的修改

## 字符串与正则表达式 ##

识别字符串的方法：
- includes()方法，在给定文本中存在于字符串中的任意位置时会返回true,否则返回false
- startsWith()方法：在给定文本出现在字符串其实处时返回true，否则返回false
- endsWith()方法：在给定文本出现在字符串结尾时返回true,否则返回false

每个方法都接收两个参数，需要搜索的文本，以及可选的搜索起始位置索引

```javascript
var	msg	=	"Hello	world!";
console.log(msg.startsWith("Hello"));	//	true
console.log(msg.endsWith("!"));		//	true
console.log(msg.includes("o"));			//	true
console.log(msg.startsWith("o"));		//	false 
console.log(msg.endsWith("world!"));	//	true 
console.log(msg.includes("x"));			//	false
console.log(msg.startsWith("o",	4));	//	true 
console.log(msg.endsWith("o",	8));	//	true 
console.log(msg.includes("o",	8));	//false
```

### repeat()方法 ###

```javascript
console.log('abc'.repeat(3)); // "abcabcabc"
```

### 模板字面量 ###

- 基本语法：使用反引号（`）来包裹普通字符串
- 若想在字符串中包含反引号，只需使用反斜杠（\）转义即可

```javascript
let message =`\`Hello\` world!`;
console.log(message); // "`Hello` world!"
```

#### 多行字符串 ####

不需要特殊的语法，只要在想要的位置包含换行即可

#### 制造替换位 ####

替换位由起始的$(name)来界定，之间允许放入任意的JS表达式

```javascript
let name = "Nicholas",				
message	= `Hello, ${name}.`;
console.log(message) ;//"Hello,	Nicholas."
```

### 函数 ###

参数默认值表达式:如果没有传参，则默认值会被调用，否则不会
```javascript
function fn(a,b=defaultValue){
	return a + b;
}
```

函数参数拥有各自的作用域和暂时性死区，与函数体的作用域相分离，这意味着参数的默认值不允许访问在函数体内部声明的任意变量

### 扩展运算符 ###

扩展运算符： ...

```javascript
let values = [23,34,45,47];
console.log(Math.max(...values)); //47
```

### 箭头函数 ###

使用一个“箭头”（=>）来定义，与传统的js函数不同
- 没有this、super、arguments，也没有new.target绑定
- 不能使用new调用，没有construct方法，不能被用作构造函数
- 没有原型，也没有prototype属性
- 不能更改this,在函数的整个生命周期内其值保持不变
- 没有arguments对象
- 不允许重复的具名参数

#### 语法 ####

- 当箭头函数只有单个参数时，可以直接书写不需要额外的语法
- 如果没有任何参数，就必须使用一对空括号
- 如果有两个参数，要加括号


### 扩展的对象功能 ###

对象类别
- 普通对象：拥有JS对象所有默认的内部行为
- 奇异对象：其内部行为在某些方面有别于默认行为
- 标准对象：在ES6中被定义的对象，例如Array、Date等
- 内置对象：在脚本开始运行时由JS运行环境提供的对象，所有标准对象都是内置对象

属性初始化器速记法
当对象的属性名称与变量名相同时，可以简单书写名称而忽略冒号与值

```javascript
{age:age}  {age}
```

方法简写
原来写法：

```javascript
var person = {
	name:"Nicholas",
	sayName:function(){
	console.log(this.name);
	} 
};

```
简写：
```javascript
var person = {
	name:"Nicholas",
	sayName(){
	console.log(this.name);
	} 
};
```

### Object.is()方法 ###

比较两个值为了避免发生强制类型转换，接收两个参数，会在两者的值相等时返回true,要求类型相同，值也相同

多数情况下,Object.is()的结果与===运算符是相同的，仅有的例外是+0与-0不相等


### Object.assign()方法 ###

将所有可枚举属性的值从一个或多个源对象复制到目标对象，返回目标对象

Object.assign(target,...sources);


## 解构 ##

### 对象解构 ###
对象解构语法在赋值语句的左侧使用了对象字面量
```javascript
let node = {
	sex:'nv',
	name:'dary'
};

let { sex, name } = node;

console.log(sex,name);
```

解构赋值

如果想在变量声明过后改变它们的值
```javascript
let node = {
	sex:'nv',
	name:'dary'
};

function out(value){
	console.log(value === node); //true
}

out({ sex, name } = node);

console.log(sex); //'nv'
console.log(name); //'dary'
```

#### 嵌套的对象解构 ####
```javascript
let node = {
	sex:'nv',
	name:'dary',
	loc:{
		start:{
			line:2
		},
		end:{
			line3:52
		}
	}
};

let { loc: { start: localStart }} = node; //重命名localStart

console.log(localStart.line); //2
```

#### 数组解构 ####

类似于对象解构，只不过将对象字面量替换成了数组字面量，解构作用在数组内部的位置上，而不是在对象的具名属性上

```javascript
let colors = ['red','green','blue'];

let [firstColor, secondColor ] = colors;

console.log(firstColor, secondColor); //"red","green"
```

如果只想取第三个元素，那么前两个可以传空。

数组解构可以很轻松的互换两个变量的值：
```javascript
let a = 1, b = 2;

[ a, b ] = [ b, a ];
 console.log(a,b);//2,1
```

嵌套的解构

```javascript
let	colors=["red",["green","lightgreen"],"blue"	];
let	[firstColor,[secondColor]]=colors;


console.log(firstColor); //red
console.log(secondColor);	//green
```

克隆数组

```javascript
let colors = ['red','green','blue'];
let [...clonedColors ] = colors;

console.log(clonedColors); //"['red','green','blue']"
```

#### 参数解构 ####

```javascript
function setCookie(name,value,
	{
		secure = false,  //默认值
		path = "/", //默认值
		domain = "baidu.com", //默认值
	} = {}
)
```

### set ###

数组去重
```javascript
let set = new Set([1,3,4,5,6,7,4,4,5,4]),
	array = [...set];

console.log(array); //[1,3,4,5,6,7]
```