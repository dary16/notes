### Promise的实例讲解
 
 了解event loop的执行顺序：
 - 一开始整个脚本作为一个宏任务执行
 - 执行过程中同步代码直接执行，宏任务进入宏任务队列，微任务进入微任务队列
 - 当前宏任务执行完出队，检查微任务列表，有则依次执行，直到全部执行完
 - 执行浏览器UI线程的渲染工作
 - 检查是否有web worker任务，有则执行
 - 执行完本轮的宏任务，回到2，依次循环，直到宏任务和微任务队列都为空
 
 **微任务包括：**MutationObserver、Promise.then()或reject()、Promise为基础开发的其它技术，比如：fetch API、V8的垃圾回收过程、Node独有的process.nextTick
 **宏任务包括：**script、setTimeout、setInterval、setImmediate、I/O、UI render
 
 **注意：**在所有任务开始的时候，由于宏任务中包括了script。所以浏览器会先执行一个宏任务，在这个过程中你看到的延迟任务如（setTimeout）将被放到下一轮宏任务中来执行
 
 #### Promise的几道基础题
 题目一：
 ```javascript
 const promise1 = new Promise((resolve,reject)=>{
  console.log('promise1');
 })
 console.log('1',promise1);
 ```
 
 过程分析：
 - 从上到下，先遇到new Promise，执行该构造函数中的代码promise1
 - 然后执行同步代码1，此时promise1没有被resolve或者reject，因此状态还是pending
 
 结果：
 ```javascript
 'promise1'
 '1' Promise{<pending>}
 ```
 
 题目二：
 ```javascript
 const promise = new Promise((resolve,reject)=>{
  console.log(1);
  resolve('success');
  console.log(2);
 });
 promise.then(()=>{
  console.log(3);
 });
 console.log(4);
 ```
 
 过程分析：
 - 从上到下，先遇到new Promise，执行其中的同步代码1
 - 再遇到resolve('success')，将promise的状态改为了resolved并且将值保存下来
 - 继续执行同步代码2
 - 跳出promise，往下执行，碰到promise.then这个微任务，将其加入微任务队列
 - 执行同步代码4
 - 本轮宏任务全部执行完毕，检查微任务队列，发现promise.then这个微任务且状态为resolved，执行它
 
 结果：
 ```javascript
 1 2 3 4
 ```
 
 题目三：
 ```javascript
 const promise = new Promise((resolve,reject)=>{
  console.log(1);
  console.log(2);
 });
 promise.then(()=>{
  console.log(3);
 });
 console.log(4);
 ```
 过程分析：
 - 和题目二相似，只不过在promise中并没有resolve或者reject
 - 因此promise.then并不会执行，它只有在被改变了状态之后才会执行
 
 结果：
 ```javascript
 1 2 4
 ```
 
 题目四：
 ```javascript
 const promise1 = new Promise((resolve,reject)=>{
  console.log('promise1');
  resolve('resolve1');
 })
 const promise2 = promise1.then(res=>{
  console.log(res);
 })
 console.log('1',promise1);
 console.log('2',promise2);
 ```
 过程分析：
 - 从上到下，先遇到new Promise，执行该构造函数中的代码promise1
 - 碰到resolve函数，将promise1的状态改变为resolved，并将结果保存下来
 - 碰到promise1.then这个微任务，将它放入微任务队列
 - promise2是一个新的状态为pending的Promise
 - 执行同步代码1，同时打印出promise1的状态是resolved
 - 执行同步代码2，同时打印出promise2的状态是pending
 - 宏任务执行完毕，查找微任务队列，发现promise1.then这个微任务且状态为resolved,执行它
 
 结果：
 ```javascript
 'promise1'
 '1' Promise{<resolved>:'resolve1'}
 '2' Promise{<pending>}
 'resolve1'
 ```
 
 题目五：
 ```javascript
 const fn = ()=>(new Promise((resolve,reject)=>{
  console.log(1);
  resolve('success')
 }))
 fn().then(res=>{
    console.log(res)
 })
 console.log('start')
 ```
 fn函数是直接返回了一个new Promise的，而且fn函数的调用是在start之前，所以它里面的内容应该会先执行
 结果：
 ```javascript
 1
 'start'
 'success'
 ```
 
 题目六：
 如果把fna的调用放到start之后呢
 ```javascript
 const fn = ()=>
   new Promise((resolve,reject)=>{
    console.log(1);
    resolve('success');
   });
   console.log('start');
   fn().then(res=>{
    console.log(res);
   })
 ```
 现在的start就在1之前打印出来了，因为fn函数是之后执行的
 
 注意：之前我们很容易就以为看到new Promise()就执行它的第一个参数函数了，其实这是不对的，就像这两道题中，我们得注意它是不是被包裹在函数中，如果是的话，只有在函数调用的时候才会执行
 
 答案：
 ```javascript
 'start'
 1
 'success'
 ```
 
 #### Promise结合setTimeout
 题目一：
 ```javascript
 console.log('start');
 setTimeout(()=>{
  console.log('time');
 })
 Promise.resolve().then(()=>{
  console.log('resolve');
 })
 console.log('end');
 ```
 
 过程分析：
 - 刚开始整个脚本作为一个宏任务来执行，对于同步代码直接压入执行栈进行执行，因此先打印出start和ens
 - setTimeout作为一个宏任务被放入宏任务队列
 - Promise.then作为一个微任务被放入微任务队列
 - 本次宏任务执行完，检查微任务，发现Promise.then，执行它
 - 接下来进入下一个宏任务，发现setTimeout，执行
 
 结果：
 ```javascript
 'start'
 'end'
 'resolve'
 'time'
 ```
 题目二：
 ```javascript
 const promise = new Promise((resolve,reject)=>{
  console.log(1);
  setTimeout(()=>{
    console.log('timerStart');
    resolve('success');
    console.log('timerEnd');
  },0)
  console.log(2);
 });
 promise.then((res)=>{
  console.log(res);
 });
 console.log(4);
 ```
 
 过程分析：
 和基础类第二题很像，不过在最外层加了setTimeout定时器
 
 - 从上至下，先遇到new Promise，执行该构造函数中的代码1
 - 然后碰到了定时器，将这个定时器中的函数放到下一个宏任务的延迟队列中等待执行
 - 执行同步代码2
 - 跳出promise函数，遇到promise.then,但其状态还是为pending，这里理解为先不执行
 - 执行同步代码4
 - 一轮循环过后，进入第二次宏任务，发现延迟队列中有setTimeout定时器，执行它
 - 首先执行timerStart，然后遇到了resolve,将promise的状态改为resolved且保存结果并将之前的promise.then推入到微任务队列
 - 继续执行同步代码timerEnd
 - 宏任务全部执行完毕，查找微任务队列，发现promise.then这个微任务，执行它
 
 结果：
 ```javascript
 1
 2
 4
 'timerStart'
 'timerEnd'
 'success'
 ```
 
 题目三：
 分了两个题目，看着差不多，实际却不一样
 （1）
 ```javascript
 setTimeout(()=>{
  console.log('timer1');
  setTimeout(()=>{
    console.log('timer3');
  },0)
 },0)
 setTimeout(()=>{
  console.log('timer2');
 },0)
 console.log('start');
 ```
 
 (2)
 ```javascript
 setTimeout(()=>{
  console.log('timer1');
  Promise.resolve().then(()=>{
    console.log('promise');
  })
 },0)
 
 setTimeout(()=>{
  console.log('timer2');
 },0)
 console.log('start');
 ```
 
 执行结果：
 ```javascript
 'start'
 'timer1'
 'timer2'
 'timer3'
 ```
 
 ```javascript
 'start'
 'timer1'
 'promise'
 'timer2'
 ```
 
 分析：
 一个是定时器timer3，一个是Promise.then
 但是如果是定时器timer3的话，它会在timer2后执行，而Promise.then却是在timer2之前执行
 
 可以这样理解，Promise.then是微任务，它会被加入到本轮的微任务列表，而定时器timer3是宏任务，它会被加入到下一轮宏任务中
 
 题目三：
 ```javascript
 Promise.resolve().then(()=>{
  console.log('promise1');
  const timer2 = setTimeout(()=>{
    console.log('timer2');
  },0)
 });
 const timer1 = setTimeout(()=>{
  console.log('timer1');
  Promise.resolve().then(()=>{
    console.log('promise2');
  })
 },0)
 console.log('start');
 ```
 
 这道题稍微难一些，在promise中执行定时器，又在定时器中执行promise
 
 并且要注意的是，这里的Promise是直接resolve的，和之前的new Promise不一样
 
 过程分析如下：
 - 刚开始整个脚本作为第一次宏任务来执行，我们将它们标记为宏1，从上到下执行
 - 遇到Promise.resolve().then这个微任务，将then中的内容加入第一次的微任务队列，标记为微1
 - 遇到定时器timer1，将它加入到下一次的宏任务延迟列表，标记为宏2，等待执行
 - 执行宏1中的同步代码start
 - 第一次宏任务（宏1）执行完毕，检查第一次微任务队列（微1）,发现有一个promise.then这个微任务要执行
 - 执行打印出微1中的同步代码promise1,然后发现定时器tiner2,将它加入宏2的后面，标记为宏3
 - 第一次微任务（微1）执行完毕，执行第二次宏任务（宏2），首先执行同步代码timer1
 - 然后遇到了promise2这个微任务，将它加入到此次循环的微任务队列，标记为微2
 - 宏2中没有同步代码可执行了，查找本次循环的微任务队列（微2），发现了promise2，执行它
 - 第二轮执行完毕，执行宏3,打印出timer2
 
 所以结果为：
 ```javascript
 'start'
 'promise1'
 'timer1'
 'promise2'
 'timer2'
 ```
 
 题目四：
 ```javascript
 const promise1 = new Promise((resolve,reject)=>{
  setTimeout(()=>{
    resolve('success')
  },1000)
 })
 
 const promise2 = promise1.then(()=>{
  throw new Error('error')
 })
 
 console.log('promise1',promise1);
 console.log('promise2',promise2);
 
 setTimeout(()=>{
  console.log('promise1',promise1);
  console.log('promise2',promise2);
 },2000)
 ```
 过程分析：
 - 从上至下，先执行第一个new Promise中的函数，碰到setTimeout将它加入到宏任务列表
 - 跳出new Promise，碰到promise1.then这个微任务，但其状态还是为pending，这里理解为先不执行
 - promise2是一个新的状态为pending的Promise
 - 执行同步代码console.log('promise1'),且打印出promise1的状态为pending
 - 执行同步代码console.log('promise2'),且打印出promise2的状态为pending
 - 碰到第二个定时器，将其放入下一个宏任务列表
 - 第一轮宏任务执行结束，并且没有微任务需要执行，因此执行第二轮宏任务
 - 先执行第一个定时器里的内容，将promise1的状态改为resolved且保存结果并将之前的promise1.then推入微任务队列
 - 该定时器中没有其他的同步代码可执行，因此执行本轮的微任务队列，也就是promise1.then，它抛出了一个错误，且将promise2的状态设置为rejected
 - 第一个定时器执行完毕，开始执行第二个定时器中的内容
 - 打印出'promise1',且此时promise1的状态为resolved
 - 打印出'promise2',且此时promise2的状态为rejected
 
 结果为：
 ```javascript
 'promise1' Promise{<pending>}
  'promise2' Promise{<pending>}
  test5.html:102 Uncaught (in promise) Error: error!!! at test.html:102
  'promise1' Promise{<resolved>: "success"}
  'promise2' Promise{<rejected>: Error: error!!!}
 ```
 
 题目五：
 ```javascript
 const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("success");
    console.log("timer1");
  }, 1000);
  console.log("promise1里的内容");
});
const promise2 = promise1.then(() => {
  throw new Error("error!!!");
});
console.log("promise1", promise1);
console.log("promise2", promise2);
setTimeout(() => {
  console.log("timer2");
  console.log("promise1", promise1);
  console.log("promise2", promise2);
}, 2000);
 ````
 
 结果：
 ```javascript
 'promise1里的内容'
'promise1' Promise{<pending>}
'promise2' Promise{<pending>}
'timer1'
test5.html:102 Uncaught (in promise) Error: error!!! at test.html:102
'timer2'
'promise1' Promise{<resolved>: "success"}
'promise2' Promise{<rejected>: Error: error!!!}
 ```
 
 
 
 
