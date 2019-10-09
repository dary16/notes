### IOS企业版部署到自己服务器（不通过AppStore,在设备上直接安装ipa） ###

#### 必须有一个https外链 ####
解决方式：找一个第三方的https外链的网盘，如阿里云的企业网盘，将plist文件放到网盘上，ipa安装包可以放到自己的服务器上

#### 不通过AppStore，在ios上直接安装企业版ipa的原理 ####
解决方式：通过itms-services协议，在safari浏览器可以直接在IOS设备上安装应用程序，itms-services协议需要一个plist配置文件，这个plist文件必须放在https服务器上，通过plist中的配置，再指向回http服务器中的ipa地址

### 部署的具体过程 ###
1.搭建一个自己的http服务器：linux下搭建基本的web服务器：https://blog.csdn.net/zhydream77/article/details/79683912
假设地址是：http://mywebserver.com
2.用企业证书打出一个pad包，并放到自己的http服务器上
企业证书打出一个ipa文件：https://blog.csdn.net/lee727n/article/details/78286178
假设打出来包的名字是：mygame.ipa
放到http服务器上，对应的下载地址假设是http://mywebserver.com/mygame.ipa
但这个并不能直接在苹果手机上下载安装，而必须通过https服务器和一个plist文件
3.申请一个https云盘（比如阿里云企业网盘） 阿里云：https://www.aliyun.com
大致流程是：
 1.购买云服务器
 2.购买OSS存储
 3.部署DzzOffice网盘
 4.连接OSS存储
假设我们申请到的地址是：https://myhttpswebserver.com
4.写一个plist文件，并放到https云上
参考下面的plist实例，注意几个地方：ipa的http地址，图标png的http地址，游戏的名字，假设我们的plist文件叫：mygame.plist
放到https服务器上，假设对应的plist文件地址是：https://myhttpswenserver.com/mygame.plist
plist文件实例
```html
<!-- mygame.plist -->

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>items</key>
    <array>
        <dict>
            <key>assets</key>
            <array>
                <dict>
                    <key>kind</key>
                    <string>software-package</string>
                    <key>url</key>
                    <string>http://mywebserver.com/mygame.ipa</string>          <!-- ipa的http地址 -->
                </dict>
                <dict>
                    <key>kind</key>
                    <string>full-size-image</string>
                    <key>needs-shine</key>
                    <false/>
                    <key>url</key>
                    <string></string>
                </dict>
                <dict>
                    <key>kind</key>
                    <string>display-image</string>
                    <key>needs-shine</key>
                    <false/>
                    <key>url</key>
                    <string>http://mywebserver.com/显示的图标.png</string>      		<!-- 显示的图标.png的http地址 -->
                </dict>
            </array>
            <key>metadata</key>
            <dict>
                <key>bundle-identifier</key>
                <string>游戏的bundleId</string>        				<!-- 游戏的bundleId, 比如com.linxinfa.mygame -->
                <key>bundle-version</key>
                <string>1.0.0</string>
                <key>kind</key>
                <string>software</string>
                <key>title</key>
                <string>游戏名字</string>   							 <!-- 游戏名字 -->
            </dict>
        </dict>
    </array>
</dict>
</plist>

```
5.写一个html下载页面，放到自己的http服务器上
假设我们的html文件叫:mygame.html
这个html页面放到服务器上，假设对应的页面地址是 http://mywebserver.com/mygame.html
html下载页示例
```html
<!-- mygame.html-->

<!doctype html> 
<html> 
<head> 
<meta charset="utf-8"> 
<meta name="viewport" content="width=device-width; initial-scale=1.0"> 
<title>iOS企业版下载测试</title> 
</head> 
<body> 

<div class="doc"> 
<p align="center"><font size="7">iOS企业版下载测试</font></p>
<p align="center">
	<!-- 这里就用到了上文提到的itms-services协议了 -->
	<a href="itms-services://?action=download-manifest&url=https://myhttpswebserver.com/mygame.plist">点击下载</a>		
</p>
</div>
</body> 
</html>

```
以上都弄好了之后，在手机safari浏览器上输入html的路径：http://mywebserver.com/mygame.html
