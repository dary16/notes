### 基础类型

#### 布尔值
最基本的数据类型就是简单的true/false值，也叫boolean
```html
let isDone: boolean = false;
```

#### 数字
同javaScript一样，typeScript里的所有数字都是浮点数。这些浮点数的类型是number。
```html
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
```

#### 字符串
javaScript程序的另一项基本操作是处理网页或服务器端的文本数据。像其它语言一样，使用string表示文本数据类型。
```html
let name:string = "bob";
```

还可以使用模板字符串，它可以定义多行文本和内嵌表达式。这种字符串是被反包围（`），并且以${expr}这种形式嵌入表达式
```html
let name: string = `Gene`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ name }.

I'll be ${ age + 1 } years old next month.`;
```

#### 数组
TypeScript像JavaScript一样可以操作数组元素。 有两种方式可以定义数组。 第一种，可以在元素类型后面接上[]，表示由此类型元素组成的一个数组：
```html
let list:number[] = [1,2,3];
```
第二种方式是数组泛型,Array<元素类型>
```html
let list:Array<number> = [1,2,3];
```

#### 元组 Tuple
元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。
```html
let x:[string,number];
x = ['hello',10];
```

#### 枚举
enum类型是对javaScript标准数据类型的一个补充。使用枚举类型可以为一组数值赋予友好的名字
```html
enum Color {Red,Green,Blue}
let c:Color = Color.Green;
```
默认情况下，从0开始为元素编号。也可以手动指定成员的数值。

#### 任意值
有时候，我们会想要为那些在变成阶段还不清楚类型的变量指定一个类型。这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。那么可以使用any类型来标记这些变量：

```html
let notSure:any = 4;
notSure = false;
```
对现有代码进行改写时，any类型是十分有用的，它允许在编译时可选择的包含或移除类型检查。

#### 空值
某种程度上来说,void类型像是与any类型相反，它表示没有任何类型。当一个函数没有返回值时，你通常会见到其返回值类型是void.
``html
function warnUser(): void {
    alert("This is my warning message");
}
```

#### NUll和Undefined
在TypeScript里，undefined和null两者各自有自己的类型分别叫做undefined和Null。
