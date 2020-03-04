### 综合练习
题目一

```javascript
const first = () => (new Promise((resolve, reject) => {
    console.log(3);
    let p = new Promise((resolve, reject) => {
        console.log(7);
        setTimeout(() => {
            console.log(5);
            resolve(6);
            console.log(p)
        }, 0)
        resolve(1);
    });
    resolve(2);
    p.then((arg) => {
        console.log(arg);
    });
}));
first().then((arg) => {
    console.log(arg);
});
console.log(4);
```

分析：
- 第一段代码定义的是一个函数，所以我们得看看它是在哪执行的，发现在4之前，所以来看看first函数里面的内容
- 函数first返回的是一个new Promise()，因此先执行里面的同步代码3
- 接着又遇到了一个new Promise(),直接执行里面的同步代码7
- 执行完7后，在p中，遇到了一个定时器，先将它放到下一个宏任务队列里不管它，接着向下走
- 碰到了resolve(1)，这里就把p的状态改为了resolved，且返回值为1,不过这里先不执行
- 跳出p ,碰到了resolve(2)，这里的resolve(2)表示的是把first函数返回的那个Promise的状态改变了，也先不管它
- 然后碰到了p.then()，将它加入本次循环的微任务列表，等待执行
- 跳出first函数，遇到了first().then()，将它加入本次循环的微任务列表
- 然后执行同步代码4
- 本轮的同步代码全部执行完毕，查找微任务列表，发现p.then和first().then()，依次执行，打印出1和2
- 本轮任务执行完毕了，发现还有一个定时器没有跑完，接着执行这个定时器里的内容，执行同步代码5
- 然后又遇到了一个resolve(6)，它是放在p里的，但是p的状态在之前已经发生过改变了，因此这里就不会再改变，也就是说resolve(6)相当于没有任何用处，因此打印出来的P为Promise{<resolved>:1}

结果：
```javascript
3
7
4
1
2
5
Promise{<resolved>: 1}
```
题目二

```javascript
const p1 = new Promise((resolve)=>{
  setTimeout(()=>{
    resolve('resolve3');
    console.log('timer1');
  },0)
  resolve('resolve1');
  resolve('resolve2');
}).then(res=>{
  console.log(res);
  setTimeout(()=>{
    console.log(p1);
  },1000)
}).finally(res=>{
  console.log('finally',res);
})
```
解析：
- Promise的状态一旦改变就无法改变
- finally不管Promise的状态是resolved还是rejected都会执行，且它的回调函数是接收不到Promise的结果的，所以finally()中的res是一个迷惑项
- 最后一个定时器打印出来的p1其实是.finally的返回值，而这道题中.finally上一个Promise是.then(),但是这个.then()并没有返回值，所以p1打印出来的Promise的值会是undefined，如果你在定时器的下面加一个return 1,则值就会变成1

结果：
```javascript
'resolve1'
'finally' undefined
'timer1'
Promise{<resolved>: undefined}
```
