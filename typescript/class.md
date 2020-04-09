### 类
先看一个例子：
```html
class Gretter{
  greeting:string;
  constructor(message:string){
    this.greeting = message;
   }
   greet(){
    return "hello,"+ this.greeting;
   }
}

let greeter = new Greeter("world");
```

我们声明一个Greeter类，这个类有3个成员：一个叫做greeting的属性，一个构造函数和一个greet方法。

我们在引用任何一个类成员的时候都用了this，它表示我们访问的是类的成员。

最后我们使用new构造了Greeter类的一个实例。它会调用之前定义的构造函数，创建一个Greeter类型的新对象，并执行构造函数初始化它。

### 继承

在TypeScript里，我们可以使用常用的面向对象模式。基于类的程序设计中一种最基本的模式是允许使用继承来扩展现有类。看下面的例子：
```html
class Animal{
  move(dis:number = 0){
    console.log(`Animal moved ${dis}m`);
  }
}

class Dog extends Animal{
  bark(){
    console.log('Woof!');
  }
}

const dog = new Dog();
dog.move(10);
```
这个例子展示了最基本的继承：类从基类中继承了属性和方法。这里,Dog是一个派生类，它派生自Animal基类，通过extends关键字。派生类通常被称作子类，基类通常被称作超类。

### 公共、私有与受保护的修饰符

#### 默认为public
在typeScript里，成员都默认为public

当成员被标记成private时，它就不能在声明它的类的外部访问。

TypeScript使用的是结构性类型系统。当我们比较两种类型的时候，情况就不同了。如果其中一个类型里包含一个private成员，那么只有当另外一个类型中也存在这样一个private成员， 并且它们都是来自同一处声明时，我们才认为这两个类型是兼容的。 对于protected成员也使用这个规则。

#### 理解protected
protected修饰符与private修饰符的行为很相似，但有一点不同，protected成员在派生类中仍然可以访问

#### readonly修饰符
可以使用readonly关键字将属性设置为只读的，只读属性必须在声明时或构造函数里被初始化

```html
class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor (theName: string) {
        this.name = theName;
    }
}
let dad = new Octopus("Man with the 8 strong legs");
dad.name = "Man with the 3-piece suit"; // 错误! name 是只读的.
```

### 静态属性
到目前为止，我们只讨论了类的实例成员，那些仅当类被实例化的时候才会被初始化的属性。 我们也可以创建类的静态成员，这些属性存在于类本身上面而不是类的实例上。 在这个例子里，我们使用static定义origin，因为它是所有网格都会用到的属性。 每个实例想要访问这个属性的时候，都要在origin前面加上类名。 如同在实例属性上使用this.前缀来访问属性一样，这里我们使用Grid.来访问静态属性。

```html
class Grid {
    static origin = {x: 0, y: 0};
    calculateDistanceFromOrigin(point: {x: number; y: number;}) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor (public scale: number) { }
}

let grid1 = new Grid(1.0);  // 1x scale
let grid2 = new Grid(5.0);  // 5x scale

console.log(grid1.calculateDistanceFromOrigin({x: 10, y: 10}));
console.log(grid2.calculateDistanceFromOrigin({x: 10, y: 10}));
```

### 抽象类

抽象类作为其它派生类的基类使用，一般不会被实例化，不同于接口，抽象类可以包含成员的实现细节。abstract关键字是用于定义抽象类和在抽象类内部定义抽象方法
```html
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earch...');
    }
}
```

### 构造函数
当你在TypeScript里声明了一个类的时候，实际上同时声明了很多东西。 首先就是类的实例的类型。

```htnl
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter: Greeter;
greeter = new Greeter("world");
console.log(greeter.greet());
```
这里，我们写了let greeter: Greeter，意思是Greeter类的实例的类型是Greeter。 这对于用过其它面向对象语言的程序员来讲已经是老习惯了。




