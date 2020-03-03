#### Promise中的then、catch、finally
 总结：
 - Promise的状态一经改变就不能再改变
 - .then和.catch都会返回一个新的Promise
 - catch不管被连接到哪里，都能捕获上层错误
 - 在Promise中，返回任意一个非Promise的值都会被包裹成promise对象，例如：return 2会被包装为return Promise.resolve(2)
 - Promise的.then或.catch可以调用多次，当如果Promise内部的状态一经改变，且有了一个值，那么后续每次调用.then或.catch的时候都会直接拿到该值
 - .then或.catch中return一个error对象并不会抛出错误，所以不会被后续的.catch捕获
 - .then或.catch返回的值不能是promise本身，否则会造成死循环
 - .then或.catch的参数期望是函数，传入的非函数则会发生值穿透
 - .then方法是能接收两个参数的，第一个是处理成功的函数，第二个是处理失败的函数，在某些时候可以认为catch是.then第二个参数的简便写法
 - .finally方法也是返回一个Promise,在Promise结束的时候，无论结果为resolve还是rejected，都会执行里面的回调函数
 
 题目一：
 ```javascript
 const promise = new Promise((resolve,reject)=>{
  resolve('success1');
  reject('error');
  resolve('success2');
 });
 promise.then(res=>{
  console.log('then:',res);
 }).catch(err=>{
  console.loh('catch:',err);
 })
 ```
 
 结果：
 ```javascript
 'then:success1'
 ```
 
 题目二：
 ```javascript
 const promise = new Promise((resolve,reject)=>{
  reject('error');
  resolve('success2');
 });
 promise.then(res=>{
  console.log('then1:',res);
 }).then(res=>{
  console.log('then2:',res);
 }).catch(err=>{
  console.log('catch:',err);
 }).then(res=>{
  console.log('then3:',res);
 })
 ```
 
 结果：
 ```javascript
 "catch: " "error"
"then3: " undefined
 ```
 
 then3会执行，那是因为catch()会返回一个Promise，且由于这个Promise没有值，所以打印出来的是undefined
 
 题目三：
 Promise.resolve(1)
 .then(res=>{
  console.log(res);
  return 2;
 })
 .catch(err=>{
  return 3;
 })
 .then(res=>{
  console.log(res);
 });
 
 结果：
 ```javascript
 1
 2
 ```
 Promise可以链式调用，不过promise每次调用.then或者.catch都会返回一个新的promise，从而实现了链式调用，它并不像一般我们任务的链式调用一样return this.
 上面的输出结果之所以依次打印出1和2，那是因为resolve(1)之后走的是第一个then方法，没有走catch里，所以第二个then中的res得到的是第一个then的返回值。
 return 2会被包装成resolve(2)
 
 题目四：
 ```javascript
 Promise.reject(1)
  .then(res=>{
    console.log(res);
    return 2;
  })
  .catch(err=>{
    console.log(err);
    return 3
  })
  .then(res=>{
    console.log(res);
  })
 ```
 结果：
 ```javascript
 1
 3
 ```
 分析：reject(1)此时走的就是catch，且第二个then中的res得到的就是catch中的返回值
 
 题目五：
 ```javascript
 const promise = new Promise((resolve,reject)=>{
  setTimeout(()=>{
    console.log('timer');
    resolve('success');
  },1000)
 })
 
 const start = Date.now();
 promise.then(res=>{
  console.log(res,Date.now() - start);
 })
 
 promise.then(res=>{
  console.log(res,Date.now() - start)
 })
 ```
 
 结果：
 ```javascript
 'timer'
 success 1001
 success 1002
 ```
 
 Promise的.then或者.catch可以被调用多次，但这里Promise构造函数只执行一次。或者说promise内部状态一经改变，并且有了一个值，那么后续每次调用.then或者.catch都会直接拿到该值
 
 题目六：
 ```javascript
 Promise.resolve().then(()=>{
  return new Error('error')
 }).then(res=>{
  console.log('then:',res);
 }).catch(err=>{
  console.log('catch:',err);
 ])
 ```
 
 结果：
 ```javascript
 'then:' 'error'
 ```
 
 分析：
 任意返回的非promise的值都会被包裹成promise对象
 
 题目七：
 ```javascript
 const promise = Promise.resolve().then(()=>{
  return promise;
 })
 promise.catch(console.err)
 ```
 
 .then或.catch返回的值不能是promise本身，否则会造成死循环
 
 题目八：
 ```javascript
 Promise.resolve(1)
 .then(2)
 .then(Promise.resolve(3))
 .then(console.log)
 ```
 第一个then和第二个then中传入的都不是函数，因此发生了穿透，将resolve(1)的值直接传到最后一个then里，所以结果为1
 
 题目九：
 .finally，记住以下知识点
 - .finally()方法不管Promise对象最后的状态如何都会执行
 - .finally()方法的回调函数不接受任何的参数，也就是说你在.finally()函数中是没法知道Promise的最终状态是resolved还是rejected
 -它最终返回的默认值是一个上一次的Promise对象值，不过如果抛出的是一个异常则返回异常的Promise对象
 
 
 
 
