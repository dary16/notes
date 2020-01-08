Audio模块用于提供音频的录制和播放功能，可调用系统的麦克风设备进行录制操作，也可调用系统的扬声器设备播放音频文件。通过plus.audio获取音频管理对象。
***
* 方法：
    getRecorder:获取当前设备的录音对象
    createPlayer:创建音频播放器对象
* 对象：
    AudioRecorder:录音对象
    AudioPlayer:音频播放对象
    AudioPlayerEvent:音频播放控件事件类型
    AudioPlayerStyles:音频播放对象的参数
    AudioRecorderStyles:音频录制的参数
* 回调方法：
    RecordSuccessCallback:录音操作成功回调
    PlaySuccessCallback:播放音频文件操作成功回调
    AudioErrorCallback:音频操作失败回调

***
* plus.audio.ROUTE_SPEAKER：设备的扬声器音频输出线路 说明：number类型，音频输出线路常量，值为0.音频播放时在设备的扬声器输出
* plus.audio.ROUTE_EARPIECE:设备听筒音频输出线路 说明：number类型，音频输出线路常量，值为1.音频播放时在设备的听筒输出

***
* plus.audio.getRecorder(); 
    说明：获取当前的录音对象，进行录音操作，录音对象是设备的独占资源，在同一时间仅可执行一个录音操作，否则可能会导致操作失败
    返回值：AudioRecorder:录音对象
    平台支持：
    * Android - 2.2+(支持)：支持录制“amr”、“3gp”、“aac”等格式
    * IOS -4.3+（支持）：支持录制“wav”、“aac”、“amr”、“MP3”等格式文件
```html


<!DOCTYPE html>
<html>
	<head>
	<meta charset="utf-8">
	<title>Audio Example</title>
	<script type="text/javascript">
// 扩展API加载完毕后调用onPlusReady回调函数 
document.addEventListener( "plusready", onPlusReady, false );
var r = null; 
// 扩展API加载完毕，现在可以正常调用扩展API 
function onPlusReady() { 
	r = plus.audio.getRecorder(); 
}
function startRecord() {
	if ( r == null ) {
		alert( "Device not ready!" );
		return; 
	} 
	r.record( {filename:"_doc/audio/"}, function () {
		alert( "Audio record success!" );
	}, function ( e ) {
		alert( "Audio record failed: " + e.message );
	} );
}
function stopRecord() {
	r.stop(); 
}
	</script>
	</head>
	<body>
		<input type="button" value="Start Record" onclick="startRecord();"/> 
		<br/>
		<input type="button" value="Stop Record" onclick="stopRecord();"/>
	</body>
</html>
	
```

***
### createPlayer:创建音频播放对象
* plus.audio.createPlayer(styles/path);
* 说明：用于播放音频文件，可以直接传入音频文件地址或音频播放参数对象。返回播放对象，可调用其play方法开始播放
* 平台支持：
    Android - 2.2+ (支持): 支持"aac"、"3gp"、"amr"、"mp3"、"mp4"、"mid"、"ogg"、"wav"等格式文件。 支持播放网络路径音频，以http/https开头，如“http://demo.dcloud.net.cn/test/audio/apple.mp3”
    
*** 
### record:调用设备麦克风进行录音操作
`recorder.record(option,successCB,errorCB)`
* 说明：调用设备麦克风开始录音操作，录音完成时调用stop方法停止。录音完成后将通过successCB 回调返回录音后的文件数据

### stop:结束录音操作
`recorder.stop();`
* 说明：结束录音操作，通知设备完成录音操作。

### AudioPlayer:音频播放对象
```javascript
interface AudioPlayer {
    function void close();
    function Boolean isPaused();
	function void play(successCB, errorCB);
	function void pause();
	function void resume();
	function void stop();
	function void seekTo(position);
	function Number getBuffered();
	function Number getDuration();
	function Number getPosition();
	function void setRoute(route);
	function void setSessionCategory(category);
}

```
* 说明：音频播放对象，用于音频文件的播放。不能通过new方法直接创建，只能通过audio.createPlayer方法创建。
* 方法：
    addEventerListener:添加事件监听
    close:关闭音频播放对象
    getBuffered:音频缓冲的时间点
    getDuration:获取音频的总长度
    getPosition:获取音频的当前播放位置
    getStyles:音频播放的参数
    isPaused:当前是否暂停或停止状态
    paly:播放音频
    pause:暂停播放
    removeEventListener:移除事件监听器
    resume:恢复播放
    seekTo:跳到指定位置
    stop:停止播放
    setRoute:音频输出线路
    setSessionCategory:设置音频播放模式
    setStyles:设置音频播放参数
    
### audioPlayerEvent:音频播放控件事件类型
常量：
* “canplay”:音频可以播放事件
* "play": 音频播放事件
* "pause":音频暂停事件
* "stop":音频停止事件
* "ended":音频自然播放结束事件
* "error":音频播放错误事件
* "waiting":音频加载中事件
* "seeking": 音频进行seek操作事件
* "seeked": 音频完成seek操作事件
* "prev": 上一曲操作事件，用户在后台播放控制器上点击上一曲按钮时触发，未开启后台控制器则不触发此事件
* "next"： 下一曲操作事件，用户在后台播放控制器上点击下一曲按钮时触发，未开启后台控制器则不触发此事件。

### AudioPlayerStyles:音频播放对象的参数
属性：
* autoplay:(Boolean类型)是否自动开始播放，默认为false
* backgroundControl:(B)是否开启后台控制器，开启后应用切换到后台可继续播放音频。ios平台在系统锁屏界面显示播放控件。android平台暂不支持
* coverImgUrl:(S)封面图地址，在后台播放控制器上显示，未开启后台则不显示
* epname:(S)专辑名，在后台播放控制器上显示，未开启后台控制器则不显示
* loop:(B)是否循环播放，默认值为false
* singer:(S)歌手名，在后台播放控制器上显示，未开启后台控制器则不显示
* src:(S)音频资源地址，支持本地路径和网络路径
* startTime:(Number)开始播放的位置，单位为秒(s)，默认为0
* title:(S) 音频标题，在后台播放器上显示，未开启后台控制器则不显示
* volume:(N) 音量，取值范围为0-1,默认值为1

### AudioRecorderStyles:音频录制参数
属性：
* channels:(S) 录制声道，可取值：“mono”-单声道录音；"stereo"-立体声道录音。默认值为'mono'
* filename:(S) 保存录音文件的路径，可设置具体文件名，也可只设置路径，如果以'/'结尾则表明是路径，文件名由录音程序自动生成。如未设置则使用默认目录生成随机文件名称，默认目录为应用%APPID%下的documents目录
* samplerate:(S) 录音文件的采样率，需通过supportedSamplerates属性获取设备支持的采样率，若设置无效的值，则使用系统默认的采样率。
* format:(S) 录音文件的格式，需通过supportedFormats属性获取设备支持的录音格式，若设置无效的值，则使用系统默认的录音格式
    平台支持：
        Android-2.2+(支持)：android平台支持“amr”、“3gp”格式，默认为"amr".
        IOS- 4.5+(支持)：iOS平台支持"wav"、"aac"、"amr"格式，默认为"wav"。






