### 设置input文本框的placeholder的颜色 ###

```html
input::-webkit-input-placeholder{
    color:rgba(144,147,153,1);
}
```

### 设置body背景色，height:100%,不生效 ###

```html
html,body{
    height:100%；
} 
或
body{
  height: 100vh; // 代表占屏幕100%
}
```

### 一像素边框问题 ###

```html
.row {
  position: relative;
  &:after{
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    width: 200%;
    border-bottom:1px solid #e6e6e6;
    color: red;
    height: 200%;
    -webkit-transform-origin: left top;
    transform-origin: left top;
    -webkit-transform: scale(0.5);
    transform: scale(0.5);
    pointer-events: none; /* 防止点击触发 */
    box-sizing: border-box;
  }
}

```

### css属性touch-action:none; ###
该属性会导致安卓页面无法滚动

### 去除ios手机端input输入框的内阴影 ###
```html
input{ 
    -webkit-appearance: none; 
}

```

### div实现背景色和背景图片同时存在 ###
```html
{
    background-color: #fff;
    background-image:url('../../assets/img/model-bg.png');
    background-repeat: no-repeat;
}

```

### css制作椭圆 ###
border-radius可以单独设置水平和垂直的半径，只需要用一个斜杠分隔这两个值就行
```html
{
    width: 150px;
    height: 100px;
    border-radius: 50%/50%; //简写属性：border-radius:50%
    background: brown;
}

```
### 图片居中显示 ###

```html
object-fit:cover;
```

### ios下取消input在输入的时候英文首字母大写 ###
```html
<input type="text" autocapitalize="none">

```

### 禁止ios识别长串数字为电话 ###
```html
<meta name="format-detection" content="telephone=no" />

```
### 禁止ios和android用户选中文字 ###
```html
-webkit-user-select: none;

```

### 一些情况下对非可点击元素监听click事件，ios下不会触发 ###
```html
cursor:politer;
```
