### link与@import的区别
- link是html方式，@import是CSS方式
- link最大限度支持并行下载，@import过多嵌套导致串行下载
- link可以通过rel="alternate stylesheet"指定候选样式
- @import必须在样式规则之前，可以在css文件中引用其他文件
- link由于@import

###清除浮动的方式，各自的优缺点
- 父级div定义height
- 结尾处加空div标签，clear:both
- 父级div定义伪类：after和zoom
- 父级div定义overflow:hidden
- 父级div也浮动，需要定义宽度
- 结尾处加br标签，clear:both

### 为什么要初始化css样式
因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对css初始化，往往会出现浏览器之间的页面显示差异

### CSS3的新特性
- 新增各种css选择器
- 圆角 border-radius
- 多列布局
- 阴影和反射
- 文字特效 text-shadow
- 线性渐变
- 旋转 transform

### CSS优先级
!important > id > class > tag

### position理解
- absolute:生成绝对定位的元素，相对于static定位以外的第一个父元素进行定位
- fixed:相对于浏览器窗口进行定位
- relative:相对于正常位置进行定位

### CSS在性能优化方面的实践
- css压缩与合并、Gzip压缩
- css文件放在head里，不要用@important
- 尽量用缩写，避免用滤镜，合理使用选择器

### base64的原理及优缺点
- 优点：可以加密，减少了HTTP请求
- 缺点：需要消耗CPU进行编辑码

### rgba()和opacity的透明效果区别
opacity作用于元素，以及元素内所有内容的透明度，rgba()只作用于元素的颜色或其背景色

###水平居中的方法
- 元素为行内元素，设置父元素 text-align:center
- 元素宽度固定：设置左右 margin:auto
- 元素为绝对定位，设置父元素 position为relative,元素设置为：left:0;right:0;margin:auto
- 使用flex-box布局，指定justify-content:center
- display:table-cell

###垂直居中的方法
- display:table-cell;vertical-align:middle
- 使用flex布局：设置为align-item:center
- 绝对定位：bottom:0;top:0;margin:auto
- 文本垂直居中 line-light = height

