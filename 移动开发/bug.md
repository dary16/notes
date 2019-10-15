1.手机出现断网，如何判断：navigator.online可判断是否是脱机状态
2.判断对象的长度：用Object.leys,返回的是一个数组，数组里装的是对象的属性
3.meta基础知识：
```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maxmum-scale=1.0,user-scalable=no" />

<!-- 设置缩放-->
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no,minimal-ui" />
<!-- 仅针对IOS的safari顶端状态条的样式（可选default/black）-->
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<!-- IOS中禁用将数字识别为电话号码，忽略安卓平台中对邮箱地址的识别 -->
<meta name="format-detection" content="telephone=no,email=no" />

<!-- window phone点击无高光 -->
<meta name="msapplication-tap-highlight" content="no" />
```
4.移动端的touch事件
//以下支持webkit
touchstart--当手指触碰到屏幕时候发生，不管当前有多少只手指
touchmove--当手指在屏幕上滑动时连续触发，通常我们在滑屏页面，会调用event的preventDefault()可以阻止默认事件的发生
页面滚动
touchend--当手指离开屏幕时触发
touchcancel--系统停止跟踪触摸时候会触发

//touchEvent说明
touched:屏幕上所有手指信息
targetTouches:手指在目标区域的手指信息
changedTouches:最近一次触发该事件的手指信息
touchend时，touches与targetTouches信息会被删除,changedTouches保存的最后一次的信息

//参数信息changedTouches[0]
clientX,clientY在显示区的坐标
target:当前元素

5.点击元素产生背景或边框怎么去掉
```html
//ios用户点击一个链接，会出现一个半透明灰色遮罩, 如果想要禁用，可设置-webkit-tap-highlight-color的alpha值为0去除灰色半透明遮罩；
//android用户点击一个链接，会出现一个边框或者半透明灰色遮罩, 不同生产商定义出来额效果不一样，可设置-webkit-tap-highlight-color的alpha值为0去除部分机器自带的效果；
//winphone系统,点击标签产生的灰色半透明背景，能通过设置<meta name="msapplication-tap-highlight" content="no">去掉；
//特殊说明：有些机型去除不了，如小米2。对于按钮类还有个办法，不使用a或者input标签，直接用div标签 
a,button,input,textarea { 
    -webkit-tap-highlight-color: rgba(0,0,0,0); 
    -webkit-user-modify:read-write-plaintext-only; //-webkit-user-modify有个副作用，就是输入法不再能够输入多个字符
}   
// 也可以 
* { -webkit-tap-highlight-color: rgba(0,0,0,0); }
//winphone下
<meta name="msapplication-tap-highlight" content="no">
```


