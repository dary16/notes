### MVVM ###

定义：是Model-View-ViewModel的缩写

- Model代表数据模型，可以在Model中定义数据修改和操作业务逻辑
- View代表UI组件，负责将数据模型转化为UI展示出来
- ViewModel监听模型数据的改变和控制视图行为、处理用户交互，同步View和Model的对象

与

### vue生命周期 ###

定义：vue实例从创建到销毁的过程，就是生命周期。从开始创建、初始化数据、编译模板、挂载DOM->渲染、更新->渲染、销毁等一系列过程。

阶段：8个阶段，创建前、后，载入前、后，更新前、后，销毁前、后

生命周期作用：有多个事件钩子，在控制整个vue实例的过程时更容易形成好的逻辑

首次页面加载会触发哪几个钩子：beforeCreate、created、beforeMount、mounted

DOM渲染在哪个周期中完成：mounted

### vue实现数据双向绑定的原理：Object.defineProperty() ###

- vue实现数据双向绑定主要是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter,getter,在数据变动时发布消息给订阅者，触发响应的监听回调。当把一个普通的javascript对象传给vue实例来作为它的data选项时，vue将遍历它的属性，用Object.definedProperty()将它们转为getter/setter.用户看不到getter/setter,但是内部它们让vue追踪依赖，在属性被访问和修改时通知变化
- vue数据双向绑定将MVVM作为数据绑定的入口，整合Observer,Compile和Watcher三者，通过Observer来监听自己的model的数据变化，通过Compile来解析编译模板指令，最终利用watcher搭起observer和Compile之间通信的桥梁，达到数据变化->视图更新；视图交互变化->数据model变更双向绑定效果

### Vue组件间参数的传递 ###

#### 父组件与子组件传值 ####
- 父传子：子组件通过props方法接收数据
- 子传父：子组件通过$emit()方法传递给父组件

#### 非父子组件的数据传递 ####

eventBus,创建一个数据总线，数据中心，相当于中转站，可以用它来传递事件和接收事件；
Vuex:相当于一个公共的存储空间
- state:存储共有数据，当组件想要用数据的时候，直接去取数据就可以了
- mutation:组件想要改变state的值必须通过mutations才能改变（mutation必须是同步的函数）
- action:类似于mutation,不同在于
	- action提交的是mutation,而不是直接变更状态
	- action可以包含任意异步操作


### Vue的路由实现：hash模式和history模式 ###

- hash模式：在浏览器中符号“#”，#以及#后面的字符称之为hash，用window.location.hash读取。特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务器安全无用，hash不会重加载页面
- history模式：history采用HTML5的新特性，提供了两个新方法：pushState(),replaceState()可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更
	- pushstate:三个参数，数据，标题，地址
	- popstate事件：读取数据 event.state

### vuex ###

- 定义：只用来读取的状态集中放在store中，改变状态的方式是提交mutations,这是个同步的过程，异步的应该封装到action中
- 使用：在main.js引入store,注入，新建一个目录store,...export导出
- 应用场景：单页应用，组件之间的状态

#### vuex: ####

- state: vuex使用单一状态树，即每个应用仅仅包含一个store实例，但单一状态树和模块化并不冲突。存放的数据状态，不可以直接修改里面的数据
- mutations:mutations定义的方法动态修改vuex中的store中的状态或数据
- getters:类似vue的计算属性，主要用来过滤一些数据
- action:actions可以理解为通过将mutations里面处理数据的方法变成可异步的处理数据的方法，就是异步操作数据

### v-if和v-show 

v-if按照条件是否渲染，v-show是display的状态

### $route和 $router的区别
- $route是“路由信息对象”,包括path,param,hash,name等路由信息参数
- $router是“路由实例”，对象包括了路由的跳转方法，钩子函数等

### <keep-alive>标签的作用

缓存组件实例，避免重新渲染

### 在vue中使用插件的步骤
- 采用ES6的import ... from ...语法或CommonJs的require()方法引入
- 使用全局方法Vue.use(plugin)使用插件，可以传入一个选项对象Vue.use(MyPlugin,{someOption:true})

### 路由之间的跳转

- 声明式：<router-link :to="path">
- 编程式：router.push(path)

### vue组件data为什么必须是函数

- 每个组件都是vue的实例
- 组件间共享data属性，当data的值是同一个引用类型的值时，改变其中一个会影响其它

### Virtual DOM 
操作DOM是很耗费性能的一件事情，通过JS对象来模拟DOM对象

### computed和watch区别

- computed是计算属性，依赖其它属性计算值，并且computed的值有缓存，只有当计算值变化才会返回内容
- watch 监听到值的变化就会执行回调，在回调中可以执行一些逻辑操作
- 一般来说需要依赖别的属性来动态获得值的时候可以使用computed，对于监听到值变化需要做一些复杂业务逻辑的情况可以使用watch


 