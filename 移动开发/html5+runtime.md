1.获取安装包版本号：plus.runtime.version
2.安装应用：plus.runtime.install(filePath, options, installSuccessCB, installErrorCB)
3.退出：plus.runtime.quit();
4.重启当前应用：plus.runtime.restart();
5.设置程序快捷方式图标上显示的角标数字：plus.runtime.setBadgeNumber(number,options);
	options:小米手机显示角标需要在系统消息中心显示一条通知，此参数用于设置通知的标题(title)和内容(content)
6.判断第三方程序是否已存在：plus.runtime.isApplicationExist(appInf);
	返回值：第三方程序已安装返回true，否则返回false
