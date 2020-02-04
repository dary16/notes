### 介绍
Promise是Javascript异步编程的一种流行解决方案

### 知识要点
- Promise 类
 Promise的构造函数必须接收一个函数参数（也就是需要执行异步任务的函数），该函数将在传入以后立即调用，并传入Promise对象下的两个方法resolve和reject
- Promise 状态
  每个Promise对象都存在以下三种 状态：
  1. PENDING:进行中，Promise对象的初始状态
  2. FULFILLED:已成功
  3. REJECTED:已失败
  每个Promise对象都只能由 PENDING 状态变成 FULFILLED 或 REJECTED，且状态发生后不能再改变
- promise.#resolve 方法
- promise.#reject 方法
- promise.then 方法
- promise.catch 方法
- promise.finally 方法（ECMA2018 added）
- promise.resolve 方法
- promise.reject 方法
- promise.all 方法
- promise.race方法

### 目标
- 了解Promise基本实现原理
- 深入掌握Promise的使用细节
- 了解Promise未来新标准新特性

### 异步任务
- 微任务
- 宏任务

then方法是一个微任务

- setTimeout(fn,0);
- postMessage-微任务

### 结果传递
在调用resolve 或者 reject方法的时候，我们还可以通过传入一些值，在后续的then 方法中，可以通过对应的函数接收到该结果
如果在一个Promise对象的状态改变后调用then则会立即执行添加的对应函数，所以需要注意必须根据当前Promise 的状态来做不同的处理
- PENDING:添加到对应的任务队列
- FULFILLED/REJECTED:不用添加到队列，而是立即执行任务

### 返回值
then方法在执行最后必须返回一个新的Promise对象
#### 重点（难点）
返回Promise对象会立即调用并执行，如果这个时候，直接去执行该对象的resolve 或者 reject 方法都会导致后续的 then 也立即被调用

我们需要对原 fulfilledHandler 和 rejectedHandler 进行包装，把它们和新 Promise 队列的resolve 和 reject 方法分别放置到新的函数中，并把这个新的函数添加到原有任务队列中调用

简单来说：把新返回的Promise 对象的 resolve 和 reject 与 then 中执行的 fulfilledHandler 和 rejectedHandler 添加到一个任务队列中执行，这样才能使用原有的 then 执行完成后才执行新的 Promise 中的 then

上面是默认情况下的处理情况，其实then方法的处理更为复杂

当一个Promise 完成(fulfilled)或者失败(rejected)，返回函数将被异步调用（由当前的线程循环来调度完成），具体的返回值依据以下规则返回：
- 如果then中的回调函数没有返回值，那么then返回的Promise将会成为接受状态，并且该接受状态的回调函数的参数为undefined
- 如果then中的回调函数返回一个值，那么then返回的Promise将会成为接受状态，并且将返回的值作为接受状态的回调函数的参数值
- 如果then中的回调函数抛出一个错误，那么then返回的Promise将会成为拒绝状态，并且将抛出的错误作为拒绝状态的回调函数的参数值
- 如果then中的回调函数返回一个已经是接受状态的Promise,那么then返回的Promise也会成为接受状态，并且将那个Promise的接受状态的回调函数的参数值作为该返回的Promise的接受状态回调函数的参数值
- 如果then中的回调函数返回一个已经是拒绝状态的Promise,那么then返回Promise也会成为拒绝状态，并且将那个Promise的拒绝状态的回调函数的参数值作为该被返回的Promise的拒绝状态回调函数的参数值
- 如果then中的回调函数返回一个未定状态(pending)的Promise，那么then返回Promise的状态也是未定的，并且它的终态与那个Promise的终态相同；同时，它变为终态时调用的回调函数参数与那个Promise变为终态时的回调函数的参数是相同的