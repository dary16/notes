API分模块封装调用了系统各种原生能力，而部分能力需要使用到Android的permissions，以下列出了各模块（或具体API）使用的的权限：
### 基础权限

5+App必须使用的到最小权限集

API | 权限 | 说明 
:-: | :-: | :-: 
ALL | <uses-permission android:name="android.permission.INTERNET"/> | 允许程序访问网络 
ALL |  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>| 允许程序读写扩展存储卡 


Audio
调用plus.audio.*使用到的权限集

API | 权限 | 说明 
:-: | :-: | :-: 
ALL |  <uses-permission android:name="android.permission.RECORD_AUDIO"/> | 允许程序录制音频 
ALL |  <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS"/>| 允许程序修改全局音频设置 


Device
调用plus.device.、plus.screen.、plus.display.、plus.networkinfo.、plus.os.*使用到的权限集

API | 权限 | 说明 
:-: | :-: | :-: 
plus.device.setWakelock(); plus.device.isWakelock(); |  <uses-permission android:name="android.permission.WAKE_LOCK"/> | 允许程序保持进程不进入休眠状态 
plus.device.vibrate(); |  <uses-permission android:name="android.permission.VIBRATE"/>| 允许程序访问振动设备 
plus.device.*  |  <uses-permission android:name="android.permission.READ_PHONE_STATE"/>  |  允许程序访问手机状态信息
plus.device.dail();  |  <uses-permission android:name="android.permission.CALL_PHONE"/>  |  允许程序不通过拨号界面拨打电话
plus.networkinfo.*  |  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>  |  允许程序访问Wi-Fi网络状态信息
plus.networkinfo.*  |  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>  |  允许程序访问有关GSM网络信息

Geolocation
调用plus.geolocation.*使用到的权限集

API | 权限 | 说明 
:-: | :-: | :-: 
ALL  |  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>  |  允许程序访问位置信息


Messaging
调用plus.messaging.*使用到的权限集

API | 权限 | 说明 
:-: | :-: | :-: 
ALL  |  <uses-permission android:name="android.permission.SEND_SMS"/>  |  允许程序发送SMS短信
ALL |   <uses-permission android:name="android.permission.READ_SMS"/>  |  允许程序读取短信息
ALL |   <uses-permission android:name="android.permission.WRITE_SMS"/> |   允许程序写短信

Barcode
调用plus.barcode.*使用到的权限集

API | 权限 | 说明 
:-: | :-: | :-: 
ALL |   <uses-permission android:name="android.permission.CAMERA"/>  |  允许程序使用照相设备
ALL  |  <uses-feature android:name="android.hardware.camera"/>  |  允许程序访问照相设备
ALL  |  <uses-feature android:name="android.hardware.camera.autofocus"/>  |  允许程序访问照相设备自动聚焦
ALL  |  <uses-permission android:name="android.permission.FLASHLIGHT"/>" |   允许程序访问闪光灯
ALL  |  <uses-permission android:name="android.permission.VIBRATE"/>  |  允许程序访问振动设备

Map
调用plus.maps.*使用到的权限集

API | 权限 | 说明 
:-: | :-: | :-: 
ALL  |  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>  |  允许程序访问CellID或WiFi热点来获取位置信息
ALL  |  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>  |  允许程序访问Wi-Fi网络状态信息
ALL  |  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>  |  允许程序访问有关GSM网络信息
ALL  |  <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>  |  允许程序改变Wi-Fi连接状态
ALL  |  <uses-permission android:name="android.permission.READ_PHONE_STATE"/>  |  允许程序访问手机状态信息
ALL |  <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/>  |  允许程序挂载和移除可移动存储设备
ALL  |  <uses-permission android:name="android.permission.READ_LOGS"/>  |  允许程序读取系统日志文件
ALL  |  <uses-permission android:name="android.permission.WRITE_SETTINGS"/>"  |  允许程序读取或写入系统设置

Payment
调用plus.payment.*使用到的权限集

API | 权限 | 说明 
:-: | :-: | :-: 
ALL  |  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>  |  允许程序访问有关GSM网络信息
ALL  |  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>  |  允许程序访问Wi-Fi网络状态信息
ALL  |  <uses-permission android:name="android.permission.READ_PHONE_STATE"/>  |  允许程序访问手机状态信息
ALL  |  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>  |  允许程序访问CellID或WiFi热点来获取位置信息

Push
调用plus.push.*使用到的权限集
个推推送

API | 权限 | 说明 
:-: | :-: | :-: 
ALL |   <uses-permission android:name="android.permission.READ_PHONE_STATE"/>  |  允许程序访问手机状态信息
ALL  |  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/> |   允许程序访问有关GSM网络信息
ALL |   <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>  |  允许程序改变Wi-Fi连接状态
ALL  |  <uses-permission android:name="android.permission.WAKE_LOCK"/>  |  允许程序保持进程不进入休眠状态
ALL  |  <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>  |  允许程序开机启动服务（离线推送服务）
ALL  |  <uses-permission android:name="android.permission.VIBRATE"/>  |  允许程序访问振动设备
ALL  |  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/> |  允许程序访问Wi-Fi网络状态信息
ALL  |  <uses-permission android:name="android.permission.GET_TASKS"/>  |  允许程序获取系统当前运行的任务信息
ALL  |  <uses-permission android:name="android.permission.READ_LOGS"/>  |  允许程序读取系统日志文件
ALL  |  <uses-permission android:name="android.permission.RECEIVE_USER_PRESENT"/>"  |  允许程序唤醒机器
ALL  |  <uses-permission android:name="getui.permission.GetuiService"/>  |  允许程序访问个推离线服务（个推自定义权限）
ALL  |  <permission android:name="getui.permission.GetuiService" android:protectionLevel="normal"/> |   允许程序访问个推离线服务（个推自定义权限）

Share
调用plus.share.*使用到的权限集
新浪微博

API | 权限 | 说明 
:-: | :-: | :-: 
ALL |   <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>  |  允许程序改变Wi-Fi连接状态
ALL |   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>  |  允许程序访问Wi-Fi网络状态信息
ALL |   <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>  |  允许程序访问有关GSM网络信息

微信

API | 权限 | 说明 
:-: | :-: | :-: 
ALL  |  <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS"/>  |  允许程序修改全局音频设置

Speech
调用plus.speech.*使用到的权限集

API | 权限 | 说明 
:-: | :-: | :-: 
ALL |   <uses-permission android:name="android.permission.RECORD_AUDIO"/>  |  允许程序录制音频
ALL |   <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>  |  允许程序访问有关GSM网络信息
ALL |   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>  |  允许程序访问Wi-Fi网络状态信息
ALL |   <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE"/>  |  允许程序改变网络连接状态
ALL |   <uses-permission android:name="android.permission.READ_PHONE_STATE"/> |   允许程序访问手机状态信息


Statistic
调用plus.statistic.*使用到的权限集

API | 权限 | 说明 
:-: | :-: | :-: 
ALL |   <uses-permission android:name="android.permission.READ_LOGS"/>  |  允许程序读取系统日志文件
ALL |   <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>  |  允许程序开机启动服务（实时提交统计数据服务）
ALL  |  <uses-permission android:name="android.permission.RECEIVE_USER_PRESENT"/>  |  允许程序唤醒机器

Native.JS
native.js封装的plus.android.* API不需要额外的权限，但导入对应对象调用native API时可能需要用到特定的权限，这时需根据情况在manifest.json中的plus->distribute->google->permissions下添加
