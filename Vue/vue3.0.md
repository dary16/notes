- composition API RFC
- 功能
	编译器(compiler)的优化主要在体现一下方面
	- 使用模块化架构
	- 优化"block tree"
	- 更激进的static tree hoisting
	- 支持Source map
	- 内置标识符前缀
	- 内置整齐打印（pretty-printing）功能
	- 移除source map和标识符前缀功能后，使用Brotli压缩的浏览器版本精简了大约10kb
	运行时（runtime）的更新主要体现在一下几个方面
		- 速度显著提升
		- 同时支持Composition API和Options API，以及typings
		- 基于Proxy实现的数据变更检测
		- 支持Fragments
		- 支持Portals
		- 支持Suspense w/async setup()
- typescript
- proxy取代defineProperty
	除了性能更高之外，还有一下缺陷，也是为啥会有$set,$delete的原因
	1.  属性的新加或者删除无法监听
	2.  数组元素的增加和删除也无法监听
- reactive模块
	看源码
	```javascript
	export function reactive(target:object){
		if(readonlyToRaw.has(target)){
			return target
		}
		if(readonlyValues.has(target)){
			return readonly(target)
		}
		return createReactiveObject(
			target,
			rawToReactive,
			reactiveToRaw,
			mutableHandlers,
			mutableCollectionHandlers
		)
	}
	```
- 关于proxy
- vue3深度检测
- vue3处理重复trigger
- 手写vue3的reactive
	- effect
	- computed
- vue3其它模块细节
 
 - compiler-core
	平台无关的编译器，它既包含可扩展的基础功能，也包含所有平台无关的插件。暴露了AST和baseCompile相关的API，它能把一个字符串变成一颗AST
 - compiler-dom
	针对浏览器的编译器
 - runtime-core
	与平台无关的运行时环境。支持实现的功能有虚拟DOM渲染器、Vue组件和Vue的各种API，可以用来自定义renderer,vue2中也有
 - runtime-dom
	针对浏览器的runtime.其功能包括处理原生DOM API、DOM属性等，暴露了重要的render和createApp方法
	```javascript
	const {render,createApp} = createRender<Node,Element>({
		patchProp,nodeOPs
	})
	export{render,createApp}
	```
 - runtime-test
	一个专门为了测试而写的轻量级runtime.比如对外暴露了renderToString方法，再次发现和react越来越像了
 - server-renderer
	用于SSR,尚未实现
 - shared
	没有暴露任何API，主要包含了一些平台无关的内部帮助方法
	