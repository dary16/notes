### 判断平台 ###
```javascript
function judgePlatform(){  
    switch ( plus.os.name ) {  
        case "Android":  
        // Android平台: plus.android.*  
        break;  
        case "iOS":  
        // iOS平台: plus.ios.*  
        break;  
        default:  
        // 其它平台  
        break;  
    }  
}
```
#### 类型转换 ####
- android原生应用的主Activity对象转为plus.android.runtimeMainActivity()
- android原生应用的主Activity对象是启动时自动创建的，不是代码创建，此时通过plus.android.runtimeMainActivity()方法获取该Activity对象
- 文件路径转换：web开发中使用的image/1.png是该web工程的相对路径，而原生API中经常要使用绝对路径，比如：/sdcard/apptest/image/1.png,此时使用这个扩展方法来完成转换：plus.io.convertLocalFileSystemURL("image/1.png")

#### 类对象 ####
由于javascript中本身没有类的概念，为了使用native API的类，在NJS引入了类对象的概念，用于对native中的类进行操作，如创建类的实力对象，访问静态属性，调用类的静态方法。其原型如下：
```javascript
Interface ClassObject{
  function Object plusGetAttribute(String name);
}
```

获取类对象
在IOS平台我们可以通过plus.ios.importClass(name)方法导入类对象，参数name为类的名称；在android平台我们可以通过plus.android.importClass(name)方法导入类对象，其参数name为类的名称：
```javascript
// iOS平台导入NSNotificationCenter类  
var NSNotificationCenter = plus.ios.importClass("NSNotificationCenter");  

// Android平台导入Intent类  
var Intent = plus.android.importClass("android.content.Intent");
```

获取类对象后，可以通过类对象“.”操作符获取类的静态常量属性、调用类的静态方法，类的静态非常量属性需通过plusGetAttribute、plusSetAttribute方法操作.

#### 实例对象 ####
在JavaScript中，所有对象都是Object，为了操作Native层类的实例对象，在NJS中引入了实例对象（InstanceObject）的概念，用于对Native中的对象进行操作，如操作对象的属性、调用对象的方法等。其原型如下：
```javascript
Interface InstanceObject {  
    function Object plusGetAttribute( String name );  
    function void plusSetAttribute( String name, Object value );  
}
```

####  获取实例对象 ####
有两种方式获取类的实例对象，一种是调用Native API返回值获取，另一种是通过new操作符来创建导入的类对象的实例，如下：
```javascript
// iOS平台导入NSDictionary类  
var NSDictionary = plus.ios.importClass("NSDictionary");  
// 创建NSDictionary的实例对象  
var ns = new NSDictionary();  

// Android平台导入Intent类  
var Intent = plus.android.importClass("android.content.Intent");  
// 创建Intent的实例对象  
var intent = new Intent();
```

获取实例对象后，可以通过实例对象“.”操作符获取对象的常量属性、调用对象的成员方法，实例对象的非常量属性则需通过plusGetAttribute、plusSetAttribute方法操作。


操作对象的属性方法
常量属性
- 获取对象后就可以通过“.”操作符获取对象的常量属性，如果是类对象则获取的是类的静态常量属性，如果是实例对象则获取的是对象的成员常量属性。
非常量属性
- 如果Native层对象的属性值在原生环境下被更改，此时使用“.”操作符获取到对应NJS对象的属性值就可能不是实时的属性值，而是该Native层对象被映射为NJS对象那一刻的属性值。
- 为获取获取Native层对象的实时属性值，需调用NJS对象的plusGetAttribute(name)方法，参数name为属性的名称，返回值为属性的值。调用NJS对象的plusSetAttribute(name,value)方法设置Native层对象的非常量属性值，参数name为属性的名称，value为要设置新的属性值。
####注意：使用plusGetAttribute(name)方法也可以获取Native层对象的常量属性值，但不如直接使用“.”操作符来获取性能高。
方法:
- 获取对象后可以通过“.”操作符直接调用Native层方法，如果是类对象调用的是Native层类的静态方法，如果是实例对象调用的是Native层对象的成员方法。
注意：在iOS平台由于JS语法的原因，Objective-C方法名称中的“:”字符转成NJS对象的方法名称后将会被忽略，因此在NJS中调用的方法名需去掉所有“：”字符。
类的继承
- Objective-C和Java中类如果存在继承自基类，在NJS中对应的对象会根据继承关系递归将所有基类的公有方法一一换成NJS对象的方法，所有基类的公有属性也可以通过其plusGetAttribute、plusSetAttribute方法访问

## 开始写NJS ##
基本步骤：
- 1.导入要使用的类
- 2.创建类的实例对象
- 3.调用实例对象的方法

以下例子使用NJS调用ios和android的原生弹出提示框

android
首先是android原生的java代码
```java
import android.app.AlertDialog;  
//...  
// 创建提示框构造对象，Builder是AlertDialog的内部类。参数this指代Android的主Activity对象，该对象启动应用时自动生成  
AlertDialog.Builder dlg = new AlertDialog.Builder(this);  
// 设置提示框标题  
dlg.setTitle("自定义标题");  
// 设置提示框内容  
dlg.setMessage("使用NJS的原生弹出框，可自定义弹出框的标题、按钮");  
// 设置提示框按钮  
dlg.setPositiveButton("确定(或者其他字符)", null);  
// 显示提示框  
dlg.show();  
//...
```
native.js代码
```javascript
/**  
 * 在Android平台通过NJS显示系统提示框  
 */  
function njsAlertForAndroid(){  
    // 导入AlertDialog类  
    var AlertDialog = plus.android.importClass("android.app.AlertDialog");  
    // 创建提示框构造对象，构造函数需要提供程序全局环境对象，通过plus.android.runtimeMainActivity()方法获取  
    var dlg = new AlertDialog.Builder(plus.android.runtimeMainActivity());  
    // 设置提示框标题  
    dlg.setTitle("自定义标题");  
    // 设置提示框内容  
    dlg.setMessage("使用NJS的原生弹出框，可自定义弹出框的标题、按钮");  
    // 设置提示框按钮  
    dlg.setPositiveButton("确定(或者其他字符)",null);  
    // 显示提示框  
    dlg.show();  
}  

```
`注意：其实HTML5+规范已经封装过原生提示框消息API：plus.ui.alert( message, alertCB, title, buttonCapture)。此处NJS的示例仅为了开发者方便理解，实际使用时调用plus.ui.alert更简单，性能也更高

IOS
以下代码在IOS平台展示调用native api系统显示提示对话框
IOS原生Object-C代码
```javascript
#import <UIKit/UIKit.h>  
//...  
// 创建UIAlertView类的实例对象  
UIAlertView *view = [UIAlertView alloc];  
// 设置提示对话上的内容  
[view initWithTitle:@"自定义标题" // 提示框标题  
    message:@"使用NJS的原生弹出框，可自定义弹出框的标题、按钮" // 提示框上显示的内容  
    delegate:nil // 点击提示框后的通知代理对象，nil类似js的null，意为不设置  
    cancelButtonTitle:@"确定(或者其他字符)" // 提示框上取消按钮的文字  
    otherButtonTitles:nil]; // 提示框上其它按钮的文字，设置为nil表示不显示  
// 调用show方法显示提示对话框，在OC中使用[]语法调用对象的方法  
[view show];  

```

native.js代码
```javascript
/**  
 * 在iOS平台通过NJS显示系统提示框  
 */  
function njsAlertForiOS(){  
    // 导入UIAlertView类  
    var UIAlertView = plus.ios.importClass("UIAlertView");  
    // 创建UIAlertView类的实例对象  
    var view = new UIAlertView();  
    // 设置提示对话上的内容  
    view.initWithTitlemessagedelegatecancelButtonTitleotherButtonTitles("自定义标题" // 提示框标题  
        , "使用NJS的原生弹出框，可自定义弹出框的标题、按钮" // 提示框上显示的内容  
        , null // 操作提示框后的通知代理对象，暂不设置  
        , "确定(或者其他字符)" // 提示框上取消按钮的文字  
        , null ); // 提示框上其它按钮的文字，设置为null表示不显示  
    // 调用show方法显示提示对话框，在JS中使用()语法调用对象的方法  
    view.show();  
}  
```
实例：在桌面创建快捷方式图标
使用NJS实现时首先导入需要使用到的android.content.Intebt、android.graphics.BitmapFactory类

```javascript
var Intent=null,BitmapFactory=null;  
var main=null;  
document.addEventListener( "plusready", function() {//"plusready"事件触发时执行plus对象的方法  
    // ...  
    if ( plus.os.name == "Android" ) {  
        // 导入要用到的类对象  
        Intent = plus.android.importClass("android.content.Intent");  
        BitmapFactory = plus.android.importClass("android.graphics.BitmapFactory");  
        // 获取主Activity  
        main = plus.android.runtimeMainActivity();  
    }  
}, false);  
/**  
 * 创建桌面快捷方式  
 */  
function createShortcut(){  
    // 创建快捷方式意图  
    var shortcut = new Intent("com.android.launcher.action.INSTALL_SHORTCUT");  
    // 设置快捷方式的名称  
    shortcut.putExtra(Intent.EXTRA_SHORTCUT_NAME, "测试快捷方式");  
    // 设置不可重复创建  
    shortcut.putExtra("duplicate",false);  
    // 设置快捷方式图标  
    var iconPath = plus.io.convertLocalFileSystemURL("/icon.png"); // 将相对路径资源转换成系统绝对路径  
    var bitmap = BitmapFactory.decodeFile(iconPath);  
    shortcut.putExtra(Intent.EXTRA_SHORTCUT_ICON,bitmap);  
    // 设置快捷方式启动执行动作  
    var action = new Intent(Intent.ACTION_MAIN);  
    action.setClassName(main.getPackageName(), 'io.dcloud.PandoraEntry');  
    shortcut.putExtra(Intent.EXTRA_SHORTCUT_INTENT,action);  
    // 广播创建快捷方式  
    main.sendBroadcast(shortcut);  
    console.log( "桌面快捷方式已创建完成！" );  
}
```
