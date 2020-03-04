#### Promise中的all和race

简单来说，.all()的作用是接收一组异步任务，然后并行执行异步任务，并且在所有异步操作执行完后才执行回调
.race()的作用也是接收一组异步任务，然后并行执行异步任务，只保留取第一个执行完成的异步操作的结果，其它的方法仍在执行，不过执行结果会被抛弃

题目一
如果直接在脚本中定义一个Promise，它构造函数的第一个参数是会立即执行的
```javascript
const p1 = new Promise(r=>console.log('立即打印'))
```
控制台中会立即打印出‘立即打印’

因此为了控制它什么时候执行，我们可以用一个函数包裹它，在需要它执行的时候，调用这个函数就可以了
```javascript
function runP1(){
  const p1 = new Promise(r=>console.log('lalla'))
  return p1
}

runP1()
```

现在来构建一个函数：
```javascript
function runAsync(x){
  const p = new Promise(r=>setTimeout(()=>r(x,console.log(x)),1000))
  return p
}
```
该函数传入一个值x，然后间隔一秒后打印出这个x

如果用.all()执行会怎么样
```javascript
function runAsync(x){
  const p = new Promise(r=>setTimeout(()=>r(x,console.log(x)),1000))
  return p
}
Promise.all([runAsync(1),runAsync(2),runAsync(3)])
.then(res=>console.log(res))
```
在间隔一秒后，控制台会同时打印出1,2,3,[1,2,3]

.all()后面的.then()里的回调函数接收的就是所有异步操作的结果，而且这个结果中的数组顺序和Promise.all()接收到的数组顺序一致

有一个场景很适用于用这个的，一些游戏类的素材比较多的应用，打开网页时，预先加载需要用到的各种资源如图片、flash以及各种静态文件。所有的都加载完后，我们再进行页面的初始化

题目二
新增了一个runReject函数，它用来在1000 * x秒后reject一个错误，同时.catch()函数能够捕获到.all()里最先的那个异常，并且只执行一次

```javascript
function runAsync (x) {
  const p = new Promise(r => setTimeout(() => r(x, console.log(x)), 1000))
  return p
}
function runReject (x) {
  const p = new Promise((res, rej) => setTimeout(() => rej(`Error: ${x}`, console.log(x)), 1000 * x))
  return p
}
Promise.all([runAsync(1), runReject(4), runAsync(3), runReject(2)])
  .then(res => console.log(res))
  .catch(err => console.log(err))
```

结果：
```javascript
1
3
//2s后输出
2
Error:2
//4s后输出
4
```
.catch()会捕获最先的那个异常，在这道题目中最先的异常就是runReject(2)的结果

题目三
.race()方法会获取最先执行完成的那个结果，其它的异步任务虽然也会继续进行下去，不过race已经不管那些任务的结果了

```javascript
function runAsync (x) {
  const p = new Promise(r => setTimeout(() => r(x, console.log(x)), 1000))
  return p
}
Promise.race([runAsync(1), runAsync(2), runAsync(3)])
  .then(res => console.log('result: ', res))
  .catch(err => console.log(err))
```
结果：
```javascript
1
'result:' 1
2
3
```
使用场景：可以用race给某个异步请求设置超时时间，并且在超时后执行相应操作

题目四：
```javascript
function runAsync(x) {
  const p = new Promise(r =>
    setTimeout(() => r(x, console.log(x)), 1000)
  );
  return p;
}
function runReject(x) {
  const p = new Promise((res, rej) =>
    setTimeout(() => rej(`Error: ${x}`, console.log(x)), 1000 * x)
  );
  return p;
}
Promise.race([runReject(0), runAsync(1), runAsync(2), runAsync(3)])
  .then(res => console.log("result: ", res))
  .catch(err => console.log(err));
```
结果：
```javascript
0
'Error:' 0
1
2
3
```

总结：
- Promise.all()的作用是接收一组异步任务，然后并行执行异步任务，并且在所有异步操作执行完后才执行回调
- .race()的作用也是接收一组异步任务，然后并行执行异步任务，只保留取第一个执行完成的异步操作的结果，其它方法仍在执行，不过执行结果会被抛弃
- Promise.all().then()结果中数组的顺序和Promise.all()接收到的数组顺序一致
