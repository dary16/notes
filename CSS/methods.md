1.使用:nth-child()选择指定元素
2.使用writing-mode排版竖文
3.使用text-align-last:justify对齐两端文本
4.使用object-fit规定图像尺寸
5.使用overflow-x排版横向列表，通过flexbox或inline-block的形式横向排列元素，对父元素设置overflow-x:auto横向滚动查看
6.通过text-overflow:ellipsis对溢出的文本在末端添加...
7.使用transform描绘1px边框:分辨率比较低的屏幕下显示1px的边框会显得模糊，通过::before或::after和transform模拟细腻的1px边框
```html
&::after {
		position: absolute;
		left: 0;
		top: 0;
		border: 1px solid $red;
		width: 200%;
		height: 200%;
		content: "";
		transform: scale(.5);
		transform-origin: left top;
	}
```
8.使用letter-spacing排版倒序文本
9.使用overflow-scrolling支持弹性滚动:iOS页面非body元素的滚动操作会非常卡(Android不会出现此情况)，通过overflow-scrolling:touch调用Safari原生滚动来支持弹性滚动，增加页面滚动的流畅度
10.使用attr()抓取data-*
11.使用:valid和:invalid校验表单
12.使用pointer-events禁用事件触发:通过pointer-events:none禁用事件触发(默认事件、冒泡事件、鼠标事件、键盘事件等)，相当于<button>的disabled
13.使用+或~美化选项框
14.使用max-height切换自动高度
15.使用filter开启悼念模式:通过filter:grayscale()设置灰度模式来悼念某位去世的仁兄或悼念因灾难而去世的人们
16.使用::selection改变文本选择颜色
17.使用linear-gradient控制背景渐变
18.使用linear-gradient控制文本渐变
19.使用caret-color改变光标颜色
20.使用:scrollbar改变滚动条样式
21.使用linear-gradient描绘波浪线
22.使用conic-gradient描绘饼图
