### 介绍
TypeScript的核心原则之一是对值所有的结构进行类型检查，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

#### 可选属性
接口里的属性不全是必须的，可选属性在应用“option bags”模式时很常用，即给函数传入的参数对象中只有部分属性赋值了。区别是在可选属性名字定义的后面加一个?符号。

#### 只读属性
一些属性只能在对象刚刚创建的时候修改其值，可以在属性名前用readonly来指定只读属性

### readonly VS const
最简单判断该用readonly还是const的方法是看要把它作为变量使用还是作为一个属性，作为变量使用的话用const，若是作为属性则用readonly.

#### 函数类型

接口能够描述javascript中对象拥有的各种各样的外形，除了描述带有属性的普通对象外，接口也可以描述函数类型。

为了使用接口表示函数类型，我们需要给接口定义一个调用签名。它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。
```html
interface SearchFunc{
  (source: string,subString:string):boolean;
}
```

#### 继承接口
和类一样，接口也可以相互继承。这样就能够从一个接口复制成员到另外一个接口里。
```html
interface Shape{
  color:string;
}

interface Square extends Shape{
  sideLength:number;
}

let square = <Square>{};
square.color = "red";
```

一个接口可以继承多个接口，创建出多个合成接口
```html
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

### 接口继承类
当接口继承了一个类类型时，它会继承类的成员但不包括其实现。就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。接口同样会继承到类的private和protected成员。这意味着当你创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现。

```html
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}

class Button extends Control implements SelectableControl {
    select() { }
}

class TextBox extends Control {

}

// Error: Property 'state' is missing in type 'Image'.
class Image implements SelectableControl {
    select() { }
}

class Location {

}
```
