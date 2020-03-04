### 使用Promise实现红绿灯交替重复亮

红灯3秒亮一次，黄灯2秒亮一次，绿灯1秒亮一次；如何让三个灯不断交替重复亮灯，三个亮灯函数已经存在
答案：
```javascript
function red() {
    console.log('red');
}
function green() {
    console.log('green');
}
function yellow() {
    console.log('yellow');
}

const light = function(timer,cb){
  return new Promise(resolve=>{
    setTimeout(()=>{
      cb()
      resolve()
    },timer)
  })
}

const step = function(){
  Promise.resolve().then(()=>{
    return light(3000,red)
  }).then(()=>{
    return light(2000,green)
  }).then(()=>{
    return light(1000,yellow)
  }).then(()=>{
    return step()
  })
}

step();
```
封装一个异步加载图片的方法
在图片的onload函数中，使用resolve返回一下就可以了

```javascript
function loadImg(url){
  return new Promise((resolve,reject)=>{
    const img = new Image();
    img.onload = function(){
      console.log('一张图片加载完成');
      resolve(img);
    };
    img.onerror = function(){
      reject(new Error('Could not load image at' + url));
    };
    img.src = url;
  })
}
```

### 限制异步操作的并发个数并尽可能块的完成全部
有8个图片资源URL，已经存储在数组urls中
urls类似于['http://1.png','http://2.png']

而且已经有一个函数function loadImg，输入url链接，返回一个Promise,图片下载完成的时候resolve,下载失败则reject

但有一个要求，任何时候同时下载的链接数量不可以超过3个
请写一段代码实现这个需求，要求尽可能快速的将所有图片下载完成

```javascript
var urls = [
  "https://hexo-blog-1256114407.cos.ap-shenzhen-fsi.myqcloud.com/AboutMe-painting1.png",
  "https://hexo-blog-1256114407.cos.ap-shenzhen-fsi.myqcloud.com/AboutMe-painting2.png",
  "https://hexo-blog-1256114407.cos.ap-shenzhen-fsi.myqcloud.com/AboutMe-painting3.png",
  "https://hexo-blog-1256114407.cos.ap-shenzhen-fsi.myqcloud.com/AboutMe-painting4.png",
  "https://hexo-blog-1256114407.cos.ap-shenzhen-fsi.myqcloud.com/AboutMe-painting5.png",
  "https://hexo-blog-1256114407.cos.ap-shenzhen-fsi.myqcloud.com/bpmn6.png",
  "https://hexo-blog-1256114407.cos.ap-shenzhen-fsi.myqcloud.com/bpmn7.png",
  "https://hexo-blog-1256114407.cos.ap-shenzhen-fsi.myqcloud.com/bpmn8.png",
];
function loadImg(url) {
  return new Promise((resolve, reject) => {
    const img = new Image();
    img.onload = function() {
      console.log("一张图片加载完成");
      resolve(img);
    };
    img.onerror = function() {
    	reject(new Error('Could not load image at' + url));
    };
    img.src = url;
  });

```

既然题目要求是保证每次开发请求的数量为3，那么我们可以先请求urls中的前面3个（下标数为0,1,2），并且请求的时候使用Promise.race()来同时请求，三个中有一个先完成了，我们就把这个当前数组中已经完成的那一项换成还没有请求的那一项。直到urls已经遍历完了，然后将最后三个没有完成的请求用Promise.all()来加载它们

```javascript
function limitLoad(urls, handler, limit) {
  let sequence = [].concat(urls); // 复制urls
  // 这一步是为了初始化 promises 这个"容器"
  let promises = sequence.splice(0, limit).map((url, index) => {
    return handler(url).then(() => {
      // 返回下标是为了知道数组中是哪一项最先完成
      return index;
    });
  });
  // 注意这里要将整个变量过程返回，这样得到的就是一个Promise，可以在外面链式调用
  return sequence
    .reduce((pCollect, url) => {
      return pCollect
        .then(() => {
          return Promise.race(promises); // 返回已经完成的下标
        })
        .then(fastestIndex => { // 获取到已经完成的下标
        	// 将"容器"内已经完成的那一项替换
          promises[fastestIndex] = handler(url).then(
            () => {
              return fastestIndex; // 要继续将这个下标返回，以便下一次变量
            }
          );
        })
        .catch(err => {
          console.error(err);
        });
    }, Promise.resolve()) // 初始化传入
    .then(() => { // 最后三个用.all来调用
      return Promise.all(promises);
    });
}
limitLoad(urls, loadImg, 3)
  .then(res => {
    console.log("图片全部加载完毕");
    console.log(res);
  })
  .catch(err => {
    console.error(err);
  });

```
