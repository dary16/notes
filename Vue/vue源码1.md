### Vue源码解析

#### 架构概览

- /compiler 目录是编译模板
- /core 目录是vue.js的核心
- /entries 目录是生产打包的入口
- /platforms 目录是针对核心模块的‘平台’模板
- platforms目录下有web和weex目录，web目录下对应的/compiler,/runtime,/server,/util目录
- /server 目录是处理服务器端渲染
- /sfc目录处理单文件 .vue
- /shared 目录提供全局用到的工具函数

结论：vue.js的组成是由core+对应的‘平台’补充代码构成（独立构建和运行时构建只是platforms下web平台的两种选择）。

vue.js的目标是通过尽可能简单的api实现响应的数据绑定和组合的视图组件。

1. compents 模板编译的代码
2. global-api 最上层的文件接口
3. instance 生命周期->init.js
4. observer数据收集与订阅
5. util 常用工具方法类
6. vdom 虚拟dom

#### 双向绑定（响应式原理）所涉及到的技术
1. Object.defineProperty
2. Observer
3. Watcher
4. Dep
5. Directive

OBSERVER

观察者模式是软件设计模式的一种。在此种模式中，一个目标对象管理所有相依于它的观察者对象，并且在它本身的状态改变时主动发出通知。这通常通过呼叫各观察者所提供的的方法来实现。此种模式通常被用来实时事件处理系统。

订阅者模式涉及三个对象：发布者、主题对象、订阅者，三个对象间是一对多的关系，每当主题对象状态发生改变时，其相关依赖对象都会得到通知，并被自动更新