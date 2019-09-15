###谷歌浏览器设置跨域
#### 49版本以后设置
方法一：
1.在电脑上新建一个目录，例如：C:\MyChromeDevUserData
2.在属性页面中的目标输入框里加上   --disable-web-security --user-data-dir=C:\MyChromeDevUserData，--user-data-dir的值就是刚才新建的目录
方法二：
1.通过桌面快捷方式打开文件位置，找到谷歌浏览器安装位置，看到chrome.exe文件
2.在电脑上新建一个目录，例如：C:\MyChromeDevUserData
3.打开cmd命令行，进入快捷方式位置，如：cd C:\Program Files (x86)\Google\Chrome\Application
4.通过命令行启动谷歌浏览器  C:\Program Files (x86)\Google\Chrome\Application>chrome.exe --disable-web-security --user-data-dir=C:\MyChromeDevUserData
