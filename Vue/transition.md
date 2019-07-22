### 过渡动画
- 在CSS过渡和动画自动应用class
- 配合第三方css动画库，如animate.css
- 在过渡钩子函数中使用javascript直接操作DOM
- 配合第三方javascript动画库，如velocity.js

#### VUE提供了transition的组件封装，在下列情形中，可以给任何元素添加进入、离开过渡
- 条件渲染（v-if）
- 条件展示(v-show)
- 动态组件
- 组件根节点

####过渡的类名
在进入、离开的过渡中，会有6个class切换
1. v-enter:定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除
2. v-enter-active:定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除
3. v-enter-to:2.1.8版及以上定义进入过渡的结束状态。在元素被插入之后下一帧生效（与此同时v-enter被移除），在过渡/动画完成之后移除
4. v-leave:定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
5. v-leave-active:定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
6. v-leave-to:2.1.8版及以上定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除


如果你使用一个没有名字的 <transition>，则 v- 是这些类名的默认前缀。如果你使用了 <transition name="my-transition">，那么 v-enter 会替换为 my-transition-enter。

使用<transition></transition>标签包裹含div为v-show或v-if的元素，指定name，如为slide-down,默认会在包裹的div上添加动画class,
如下：
```html
&.slide-down-enter, &.slide-down-leave-to{transform:translated3d(0,-100%,0)}
&.slide-down-enter-to, &slide-down-leave{transform:translated3d(0,0,0)}
&.slide-down-enter-active, &.slide-down-leave-active{transition:all .3s linear;}
```
