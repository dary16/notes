###安装Node.js

1. node官网下载安装包，选择LTS版本 
2. 安装，比如我安装到d:/node文件下，安装时默认安装在c盘
3. 在刚才安装node的文件夹内创建两个文件夹：node_cache和node_global
4. 设置全局目录和缓存目录，打开cmd窗口，输入：npm config set prefix "D:\node\node_global";npm config set cache "D:\node\node_cache"
5. 在C盘搜索.npmrc打开看是否和刚才设置是一样的
6. 配置环境变量：

电脑右键->属性->高级系统设置->环境变量
用户变量：path新建一条，为node_global的路径，如：D:\node\node_global
系统变量：新建NODE_PATH：安装node是node_modules的路径，如D:\node\node_modules

安装cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org

如果报错，可以运行下面命令再试
- npm set registry https://registry.npm.taobao.org
- npm set disturl https://npm.taobao.org/dist
- npm install -g cnpm --registry=https://registry.npm.taobao.org

成功后用cnpm -v测试
卸载vue2的版本报错，记住只要配node的环境变量，不需要配置npm的！！
安装vue-cli3:npm install -g @vue/cli
安装webpack:cnpm install -g webpack