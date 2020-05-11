#### webpack可视化分析

进行webpack优化打包，先来分析一下webpack打包性能瓶颈，此时要用到webpack-bundle-analyzer
1. 安装依赖
```html
npm install webpack-bundle-ana
```

2. 在vue.config.js配置
```javascript
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer')
configureWebpack:(config)=>{
  if(process.env.NODE_ENV === 'production'){
    config.plugins.push(new BundleAnalyzerPlugin())
  }
}
```

打包后，可以看到一份依赖图，分析可以得到一下信息：
- 打包出的文件中都包含了什么，以及模块之间的依赖关系
- 每个文件的大小在总体中的占比，找出较大的文件，思考是否有替换方案，是否使用了它包含了不必要的依赖？
- 是否有重复的依赖项，对此可以如何优化？
- 每个文件的压缩后的大小。
