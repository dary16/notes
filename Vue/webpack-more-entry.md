### 多入口文件如何开发 ###
生成多个html-webpack-plugin实例来解决问题

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    mode:'development', // 开发模式
    entry: {
      main:path.resolve(__dirname,'../src/main.js'),
      header:path.resolve(__dirname,'../src/header.js')
  }, 
    output: {
      filename: '[name].[hash:8].js',      // 打包后的文件名称
      path: path.resolve(__dirname,'../dist')  // 打包后的目录
    },
    plugins:[
      new HtmlWebpackPlugin({
        template:path.resolve(__dirname,'../public/index.html'),
        filename:'index.html',
        chunks:['main'] // 与入口文件对应的模块名
      }),
      new HtmlWebpackPlugin({
        template:path.resolve(__dirname,'../public/header.html'),
        filename:'header.html',
        chunks:['header'] // 与入口文件对应的模块名
      }),
    ]
}


```

每次执行npm run build会发现dist文件夹里会残留上次打包的文件，推荐一个plugin来帮忙在打包输出前清空文件夹
```javascript
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
module.exports = {
    // ...省略其他配置
    plugins:[new CleanWebpackPlugin()]
}

```

### 引用css ###

需要一些loader来解析我们的css文件
npm i -D style-loader css-loader
如果要用less来构建样式，则需要多安装两个
npm i -D less less-loader

配置文件如下
```javascript
// webpack.config.js
module.exports = {
    // ...省略其他配置
    module:{
      rules:[
        {
          test:/\.css$/,
          use:['style-loader','css-loader'] // 从右向左解析原则
        },
        {
          test:/\.less$/,
          use:['style-loader','css-loader','less-loader'] // 从右向左解析原则
        }
      ]
    }
} 

```

