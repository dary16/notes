### async/await讲解练习

题目一
```javascript
async function async1(){
  console.log('async1 start');
  await async2();
  console.log('async1 end');
}
async function async2(){
  console.log('async2');
}
async1();
console.log('start');
```
结果：
```javascript
'async1 start'
'async2'
'start'
'async1 end'
```

分析：
- 首先一进来创建了两个函数，先看调用的位置
- 发现async1函数被调用了，然后去看看调用的内容
- 执行函数中的同步代码async1 start,之后碰到了await，它会阻塞async1后面的代码执行，因此会先去执行async2中的同步代码async2,然后跳出async1
- 跳出async1函数后，执行同步代码start
- 在一轮宏任务全部执行完之后，再来执行刚刚await后面的内容async1 end

可以理解为await后面的内容就相当于放到了Promise.then的里面

看一下就await转换为Promise.then的伪代码
```javascript
async function async1() {
  console.log("async1 start");
  // 原来代码
  // await async2();
  // console.log("async1 end");
  
  // 转换后代码
  new Promise(resolve => {
    console.log("async2")
    resolve()
  }).then(res => console.log("async1 end"))
}
async function async2() {
  console.log("async2");
}
async1();
console.log("start")
```
另外关于await和Promise的区别，如果把await async2()换成一个new Promise呢
```javascript
async function async1(){
  console.log('async1 start');
  new Promise(resolve=>{
    console.log('promise');
  })
  console.log('async1 end');
}
async1();
console.log('start');
```
结果：
```javascript
'async start'
'promise'
'async1 end'
'start'
```
new Promise()并不会阻塞后面的同步代码执行

题目二

将async结合定时器看看
```javascript
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}
async function async2() {
  setTimeout(() => {
    console.log('timer')
  }, 0)
  console.log("async2");
}
async1();
console.log("start")
```
结果：
```javascript
'async1 start'
'async2'
'start'
'async1 end'
'timer'
```

题目三
正常情况下，async和await命令是一个Promise对象，返回该对象的结果，但是如果不是Promise对象的话，就会直接返回对应的值
```javascript
async function fn(){
  return 123
}
fn().then(res=>console.log(res))
```
结果：123

题目四

```javascript
async function async1(){
  console.log('async1 start');
  await new Promise(resolve =>{
    console.log('promise1');
  })
  console.log('async1 success');
  return 'async1 end'
}
console.log('script start');
async1().then(res=>console.log(res))
console.log('script end');
```
在async1中await后面的Promise是没有返回值的，也就是说它的状态始终是pending,所以await之后的内容是不会执行的，也包括async1后面的.then
结果：
```javascript
'script start'
'async1 start'
'promise1'
'script end'
```

题目五
给上面的题的Promise加上resolve
```javascript
async function async1(){
  console.log('async1 start');
  await new Promise(resolve=>{
    console.log('promise1');
    resolve('promise1 resolve');
  }).then(res=>console.log(res))
  console.log('async1 success');
  return 'async1 end'
}
console.log('script start');
async1().then(res=>console.log(res))
console.log('script end');
```
现在Promise有了返回值了，因此await后面的内容将会被执行
```javascript
'script start'
'async1 start'
'promise1'
'script end'
'promise1 resolve'
'async1 success'
'async1 end'
```
题目六

```javascript
async function async1(){
  console.log('async1 start');
  await new Promise(resolve=>{
    console.log('promise1');
    resolve('promise resolve');
  })
  console.log('async1 success');
  return 'async end'
}
console.log('script start');
async1().then(res=>{
  console.log(res);
})
new Promise(resolve=>{
  console.log('promise2');
  setTimeout(()=>{
    console.log('timer')
  })
})

```
在async1中的new Promise 它的resolve的值和async1().then()里的值没有关系的
结果：
```javascript
'script start'
'async1 start'
'promise1'
'promise2'
'async1 success'
'sync1 end'
'timer'
```
题目七

```javascript
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}

async function async2() {
  console.log("async2");
}

console.log("script start");

setTimeout(function() {
  console.log("setTimeout");
}, 0);

async1();

new Promise(function(resolve) {
  console.log("promise1");
  resolve();
}).then(function() {
  console.log("promise2");
});
console.log('script end')
```
结果：
```javascript
'script start'
'async1 start'
'async2'
'promise1'
'script end'
'async1 end'
'promise2'
'setTimeout'
```

### async处理错误
题目一
在async中，如果await后面的内容是一个异常或者错误的话，会怎么样呢
```javascript
async function async1(){
  await async2();
  console.log('async1');
  return 'async1 success'
}
async function async2(){
  return new Promise((resolve,reject)=>{
    console.log('async2');
    reject('error');
  })
}
async1().then(res=>console.log(res))
```
如果在async函数中抛出了错误，则终止错误结果，不会继续向下执行
结果：
```javascript
'async2'
Uncaught （in promise） error
```
题目二
如果想要使得错误的地方不影响async函数后续执行的话，可以使用try catch
```javascript
async function async1 () {
  try {
    await Promise.reject('error!!!')
  } catch(e) {
    console.log(e)
  }
  console.log('async1');
  return Promise.resolve('async1 success')
}
async1().then(res => console.log(res))
console.log('script start')
```
结果：
```javascript
'script start'
'error!!!'
'async1'
'async1 success'
```
