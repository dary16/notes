### JSON数据格式解析

定义：JSON(JavaScript Object Notation)是一种轻量型的数据交换格式，易于人阅读和编写，更利于机器的解析和生成

#### JSON语法规则
在JS中，一切都是对象，因此，任何支持的类型都可以通过JSON来表示，例如字符串、数字、对象、数组等，对象和数组是比较特殊且常用的两种类型：
- 对象表示为键值对
- 数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

##### JSON键/值对
JSON键值对是用来保存JS对象的一种方式，和JS对象的写法也大同小异，键值对组合中键名写在前面并用双引号""包裹，使用冒号:分隔，然后紧接着值
eg:{"firstName":"json"}

#####JSON与JS对象的关系：JSON是JS对象的字符串表示法，它使用文本表示一个JS对象的信息，本质是一个字符串，如：
```javascript
var obj={a:'hello',b:'world'};//这是一个对象，注意键名也可以使用引号包裹的
var json='{"a":"hello","b":"world"}';//这是一个JSON字符串，本质是一个字符串
```

#####JSON和JS对象互转
要实现从JSON字符串转换为JS对象，使用JSON.parse()方法：
```javascript
var obj = JSON.parse('{"a":"hello","b":"world"}');//结果是{a:"hello",b:"world"}
```
要实现从JS对象转换为JSON字符串，使用JSON.stringify()方法：
```javascript
var json = JSON.stringify({a:'hello',b:'world'});//结果是'{"a":"hello","b":"world"}'
```

#####常用类型
任何支持的类型都可以通过JSON来表示，例如字符串、数字、对象、数组等。但是对象和数组是比较特殊且常用的类型。
