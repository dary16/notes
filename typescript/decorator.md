### 装饰器

装饰器最早在Python中被引入，它的作用是给一个已有的方法或类扩展一些新的行为，而不是去直接修改它本省。

在ES2015进入class之后，当我们需要在多个不同的类之间共享或者扩展一些方法或行为的时候，代码会变得错综复杂，及其不优雅，这也是装饰器被提出的一个重要原因。

在Javascript中我们需要Babel插件babel-plugin-transform-decorators-legacy来支持decorator，而在Typescript中我们需要在tsconfig.json里面开启支持选项experimentalDecorators.


先明确两个概念：
- 目前装饰器本质是一个函数,@expression的形式其实是一个语法糖，expression求值后必须也是一个函数，它会在运行时被调用，被装饰的声明信息作为参数传入
- JavaScript中的Class其实也是一个语法糖


#### 类装饰器

我们声明一个函数addAge去给Class的属性age添加年龄

```html
funtion addAge(constructor:Function){
 constructor.prototype.age = 18;
}

@addAge
class Person{
  name:string;
  age!:number;
  constructor(){
   this.name = 'dary';
 }
}

let person = new Person();

console.log(person.age);//18
```

当装饰器作为修饰类的时候，会把构造器传递进去。

#### 属性/方法装饰器
一个Class的属性/方法也可以被装饰，我们分别给Person类加上say和run 方法：

```html
//声明装饰器修饰方法/属性
function method(target:any,propertyKey:string,descriptor:PropertyDescriptor){
   console.log(target);
   console.log("prop " + propertyKey);
   console.log("desc " + JSON.stringify(descriptor) + "\n\n");
   descriptor.writable = false;
}

class Person{
 name:string;
 constructor(){
  this.name = 'dary';
 }

  @method
  say(){
   return 'instance method';
  }

   @method
   static run(){
    return 'static method';
  }
}

const xmz =  new Person();
```

#### 小结：
虽然装饰器目前依然在 TC39 的草案阶段,但是其实他已经借助 Babel 或者 TypeScript 广泛运用于各种业务开发或者基础库中,这就得益于它强大的抽象与重用特性,比如 Angular 中就大量运用了装饰器,但是仅仅借助装饰器的力量是不够的,我们知道在 Java 中有与装饰器非常像的一种语法叫注解,这就不得不提 Reflect Metadata了.
