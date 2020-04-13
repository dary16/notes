### 泛型是TypeScript中非常重要的概念，愿意就在于泛型给与开发者创造灵活、可重用代码的能力

适用于情况：在静态编写时不确定传入的参数到底是什么类型，只要当运行时传入参数后才能确定。我们需要变量，这个变量代表了传入的类型，它是一种特殊的变量，只用于表示类型而不是值，这个类型变量在TypeScript中叫做泛型。

```html
function returnItem<T>(para:T):T{
  return para
}
```
在函数名称后面声明泛型变量<T>，它用于捕获开发者传入的参数类型，然后就可以使用T做参数类型和返回值类型。

#### 多个类型参数
定义泛型的时候，可以一次性定义多个类型参数，比如同时定义泛型T和泛型U:
```html
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
```
#### 泛型接口

泛型也可以用于接口声明，如下：
 ```html
 interface ReturnItemFn<T> {
    (para: T): T
}
 ```
 
 #### 泛型类
 泛型除了可以在函数中使用，还可以在类中使用，它既可以作用于类本身，也可以作用与类的成员函数。
 
 ```html
 class Stack {
    private arr: number[] = []

    public push(item: number) {
        this.arr.push(item)
    }

    public pop() {
        this.arr.pop()
    }
}
 ```
 ### 小结：
 设计泛型的关键目的是在成员之间提供有意义的约束，这些成员可以是:
 - 接口
 - 类的实例成员
 - 类的方法
 - 函数参数
 - 函数的返回值
 
