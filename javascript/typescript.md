### 原始数据类型正确的写法：
```javascript
//布尔值
let isDone:boolean = false;

//数值
let decLiteral:number = 6;
let hexLiteral:number = 0xf00d;

//字符串
let myName:string = 'Dary';

//空值:没有返回值的函数为void
function alertName(): void{
  alert('my name is Dary');
}

//undefined类型的变量只能被赋值为undefined,null类型的变量只能被赋值为null
let u:undefined = undefined;
let n:null = null;

//任意值：可以被任何值赋值
let anyThing: any = 'hello';
let anyThing: any = 666;

//联合类型：表示取值可以是多种类型中的一种
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

//也可以用于方法的参数定义，都有toString方法，访问string和number的共有属性是没问题的
function getString(something:string | number):string{
  return something.toString();
}
```

#### 对象的类型————接口
```javascript
//赋值的时候，变量的形状必须和接口的形状保持一致
interface Person{
  name:string;
  age:number;
}

let tom:Person = {
  name:'dary';
  age:25
};

IUserInfo{
  age:any;
  userName:string
}

function getUserInfo(user:IUserInfo):string{
  return user.age+"======"+user.userName;
}

//可选属性
interface Person{
  name:string;
  age?:number;//表示这个属性可有可无
}

let tom:Person = {
  name:'Tom'
}

//任意属性
//希望一个接口允许有任意的属性，一旦定义了任意属性，那么确定属性和可选属性的类型必须是它的类型的子集
interface Person{
  name:string;
  age?:number;
  [propName:string]:any;
}

let tom:Person = {
  name:'Tom';
  gender:'male'
}

//只读属性
interface Person {
    readonly id: number; // 
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757, // 只读
    name: 'Tom',
    gender: 'male'
};


```

//数组的类型
```javascript
let fibonacci:number[]=[1,1,2,3,5];
let fibonacci:Array<number> = [1,1,2,3,5];
```

//函数的类型
```javascript
//需要把输入和输出都考虑到
function sum(x:number,y:number):number{
  return x + y;
}

//函数表达式
let mySum = function(x:number,y:number):number{
  return x + y;
}

//不要混淆了TypeScript中的=> 和ES6中的=>
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};

//接口定义函数的形状
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source, subString) {
    return source.search(subString) !== -1;
}

//可选参数
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');



```
