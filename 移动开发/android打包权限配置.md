### 基础权限 ###
Android权限配置说明


API权限


| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.INTERNET"/&gt; | 允许程序访问网络 |
| &lt;uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/&gt; | 允许程序读写扩展存储卡 |

Audio
调用plus.audio.*使用到的权限集


| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.CAMERA"/&gt; | 允许程序使用照相设备 |
| &lt;uses-feature android:name="android.hardware.camera"/&gt; | 允许程序访问照相设备 |

Contacts
调用plus.contacts.*使用到的权限集


| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.GET_ACCOUNTS"/&gt; | 允许程序访问Accounts Service账户列表 |
| &lt;uses-permission android:name="android.permission.READ_CONTACTS"/&gt; | 允许程序读取用户联系人数据 |
| &lt;uses-permission android:name="android.permission.WRITE_CONTACTS"/&gt; | 允许程序修改用户联系人数据 |

Device
调用plus.device、plus.screen、plus.display、plus.networkinfo、plus.os.* 使用到的权限集


| API | 权限 | 说明 |
| ------ | ------ | ------ |
| plus.device.setWakelock();plus.device.isWakelock(); |	&lt;uses-permission android:name="android.permission.WAKE_LOCK"/&gt; | 允许程序保持进程不进入休眠状态 |
| plus.device.vibrate(); | &lt;uses-permission android:name="android.permission.VIBRATE"/&gt; | 允许程序访问振动设备 |
| plus.device.* | &lt;uses-permission android:name="android.permission.READ_PHONE_STATE"/&gt; | 允许程序访问手机状态信息 |
| plus.device.dail(); | &lt;uses-permission android:name="android.permission.CALL_PHONE"/&gt; | 允许程序不通过拨号界面拨打电话 |
| plus.networkinfo.* | &lt;uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/&gt; | 允许程序访问Wi-Fi网络状态信息 |
| plus.networkinfo.* | &lt;uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/&gt; | 允许程序访问有关GSM网络信息 |

Geolocation
调用plus.messageing.*使用到的权限集


| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/&gt; | 允许程序访问位置信息 |

Messageing
调用plus.messageing.*使用得到的权限集


| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.SEND_SMS"/&gt; | 允许程序发送SMS短信 |
| &lt;uses-permission android:name="android.permission.READ_SMS"/&gt; | 允许程序读取短信息 |
| &lt;uses-permission android:name="android.permission.WRITE_SMS"/&gt; | 允许程序写短信 |

Barcode
调用plus.barcode.*使用到的权限集

| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.CAMERA"/&gt; | 允许程序使用照相设备 |
| &lt;uses-permission android:name="android.hardware.camera"/&gt; | 允许程序访问照相设备 |
| &lt;uses-permission android:name="android.hardware.camera.autofocus"/&gt; | 允许程序访问照相设备自动聚焦 |
| &lt;uses-permission android:name="android.permission.FLASHLIGHT"/&gt; | 允许程序访问闪光灯 |
| &lt;uses-permission android:name="android.permission.VIBRATE"/&gt; | 允许程序访问振动设备 |

Map
调用plus.maps.*使用得到的权限集

| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/&gt; | 允许程序访问CellID或WiFI热点来获取位置信息 |
| &lt;uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/&gt; | 允许程序访问WIFI网络状态信息 |
| &lt;uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/&gt; | 允许程序访问有关GSM网络信息 |
| &lt;uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/&gt; | 允许程序改变Wi-Fi连接状态 |
| &lt;uses-permission android:name="android.permission.READ_PHONE_STATE"/&gt; | 允许程序访问手机状态信息 |
| &lt;uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/&gt; | 允许程序挂载和移除存储设备 |
| &lt;uses-permission android:name="android.permission.READ_LOGS"/&gt; | 允许程序读取系统日志文件 |
| &lt;uses-permission android:name="android.permission.WRITE_SETTINGS"/&gt; | 允许程序读取或写入系统设置 |


Payment
调用plus.payment.*使用得到的权限集

| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/&gt; | 允许程序访问有关GSM网络信息 |
| &lt;uses-permission android:name="android.permission.ACCESS.WIFI_STATE"/&gt; | 允许程序访问WI-FI网络状态信息 |
| &lt;uses-permission android:name="android.permission.READ_PHONE_STATE"/&gt; | 允许程序访问手机状态信息 |
| &lt;uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/&gt; | 允许程序访问cellID或WiFI热点来获取位置信息 |


Push
调用plus.push.*使用到的权限集

个推推送

| API权限 | 说明 |
| ------ | ------ |
| &lt;uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/&gt; | 允许程序访问WIFI网络状态信息 |
| &lt;uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/&gt; | 允许程序访问有关GSM网络信息 |
| &lt;uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/&gt; | 允许程序改变Wi-Fi连接状态 |
| &lt;uses-permission android:name="android.permission.READ_PHONE_STATE"/&gt; | 允许程序访问手机状态信息 |
| &lt;uses-permission android:name="android.permission.WEAK_LOCK"/&gt; | 允许程序保持进程不进入休眠状态 |
| &lt;uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/&gt; | 允许程序开机启动服务（离线推送服务） |
| &lt;uses-permission android:name="android.permission.VIBRATE"/&gt; | 允许程序访问振动设备 |
| &lt;uses-permission android:name="android.permission.GET_TASKS"/&gt; | 允许程序获取系统当前运行的任务信息 |
| &lt;uses-permission android:name="android.permission.READ_LOGS"/&gt; | 允许程序读取系统日志文件 |
| &lt;uses-permission android:name="getui.permission.GetuiService" android:protectionLevel="normal"/&gt; | 允许程序访问个推离线服务（个推自定义权限） |
| &lt;uses-permission android:name="android.permission.RECEIVE_USER_PRESENT"/&gt; | 允许程序唤醒机器 |
| &lt;uses-permission android:name="getui.permission.GetuiService"/&gt; | 允许程序访问个推离线服务（个推自定义权限） |


Share
调用plus.share.*使用到的权限集

新浪微博

| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/&gt; | 允许程序改变Wi-Fi连接状态 |
| &lt;uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/&gt; | 允许程序访问Wi-Fi网络状态信息 |
| &lt;uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/&gt; | 允许程序访问有关GSM网络信息 |

腾讯微博

| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/&gt; | 允许程序改变Wi-Fi连接状态 |
| &lt;uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/&gt; | 允许程序访问有关GSM网络信息 |
| &lt;uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/&gt; | 允许程序访问CellID或WiFi热点来获取位置信息 |
| &lt;uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/&gt; | 允许程序访问位置信息 |
| &lt;uses-permission android:name="android.permission.ACCESS_MOCK_LOCATION"/&gt; | 允许程序创建模拟位置 |

微信

| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS"/&gt; | 允许程序修改全局音频设置 |

Speech

调用plus.speech.*使用到的权限集

| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.READ_AUDIO"/&gt; | 允许程序录制音频 |
| &lt;uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/&gt; | 允许程序访问有关GSM网络信息 |
| &lt;uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/&gt; | 允许程序访问WI-FI网络状态信息 |
| &lt;uses-permission android:name="android.permission.CHANGE_NETWORK_STATE"/&gt; | 允许程序改变网络连接状态 |
| &lt;uses-permission android:name="android.permission.READ_PHONE_STATE"/&gt; | 允许程序访问手机状态信息 |

Statistic

调用plus.statistic.*使用到的权限集

| API权限 | 说明 |
| ------ | ------ |
|	&lt;uses-permission android:name="android.permission.READ_LOGS"/&gt; | 允许程序读取系统日志文件 |
| &lt;uses-permission android:name="android.permission.RECEIVE_BOOT_COM"/&gt; | 允许程序开机启动服务（实时提交统计数据服务） |
| &lt;uses-permission android:name="android.permission.RECEIVE_USER_PRESENT"/&gt; | 允许程序唤醒机器 |
