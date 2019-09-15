flex布局
容器属性：display属性用来将父元素定义为Flex布局的容器，设置display值为display：flex容器对外表现为块级元素；display：inline-flex，容器对外表现为行内元素，对内两者表现是一样的。
我们有以下六个属性可以设置在容器上：
1.flex-direction定义了主轴的方向，即项目的排列方向
  row(默认值)：主轴在水平方向上，起点在左侧，也就是我们常见的从左向右
  row-reverse：主轴水平方向，起点在右侧
  column：主轴在垂直方向，起点在上沿
  column-reverse：主轴在垂直方向
2.flex-wrap，默认情况下，项目是排成一行显示的，flex-wrap用来定义当一行放不下时，项目如何操作
  nowrap（默认）：不换行
  wrap： 换行，第一行在上面
  wrap-reverse：换行，第一行在下面
3.flex-flow是flex-direction和flex-wrap的简写，默认值为row no-wrap
4.justify-content 定义了项目在主轴上的对齐方式。
  flex-start（默认）：与主轴的起点对齐；
  flex-end：与主轴的终点对齐；
  center：项目居中；
  space-between：两端对齐，项目之间的距离都相等；
  space-around：每个项目的两侧间隔相等，所以项目与项目之间的间隔是项目与边框之间间隔的两倍
5.align-items 定义了项目在交叉轴上如何对齐
  flex-start：与交叉轴的起点对齐； flex-end：与交叉轴的终点对齐； center：居中对齐； baseline：项目第一行文字的基线对齐； stretch（默认值）：如果项目未设置高度或者为 auto，项目将占满整个容器的高度。
6.align-content 定义了多根轴线的对齐方式，若此时主轴在水平方向，交叉轴在垂直方向，align-content 就可以理解为多行在垂直方向的对齐方式。项目排列只有一行时，该属性不起作用
参考：https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox
