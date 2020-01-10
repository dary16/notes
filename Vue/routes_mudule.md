### 拆分路由
根据不同的业务模块进行拆分路由，在router文件夹下建不同的文件夹，里面对应index.js
在每个子模块中导出一个路由配置数组
```javascript
export default[
  {
    path:'/news',
    component:()=> import('@/views/news/index.vue')
  }
]
```

在根Index.js中导入所有子模块
```javascript
//router/index.js
import Vue from 'vue'
import Router from 'vue-router'
import newRouterConfig from './news'
import productRouterConfig from './product'

Vue.use(Router)

const routes = [...newRouterConfig,...productRouterConfig]

export default new Router({
  mode:'history',
  base:process.env.BASE_URL,
  routes:routes
})
```

### 自动扫描模块路由并导入
当我们的业务越来越大，每次新增业务模块的时候，我们都要在路由下面新增一个子路由模块，然后在Index.js中导入。
如何简化呢？
通过自动扫描全局组件注册，我们也可以实现自动扫描子模块路由并导入
```javascript
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

let routes = []

const routerContext = require.context('./',true,/index\.js$/)
routerContext.keys().forEach(route=>{
  //如果是根目录的index.js,不处理
  if(route.startsWith('./index')){
    return
  }
  const routerModule = routerContext(route)
  //兼容Import export 和require module.export两种规范
  routes = [...routes,...(routerModule.default || routerModule)]
})

export default new Router({
  mode:'history',
  base:process.env.BASE_URL,
  routes:routes
})
```

