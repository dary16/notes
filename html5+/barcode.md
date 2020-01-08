    BarCode模块管理条码（一维码和二维码）扫描识别，支持常见的一维码及二维码。通过调用设备的摄像头对条码进行扫描识别，扫描到条码后进行解码并返回码数据内容及码类型。
    BarCode模块可使得web开发人员能快速方便调用设备的摄像头进行条码扫描识别，而不需要安装额外的扫描插件。规范建议条码识别引擎的支持规范定义的所有条码常量类型。
    常量：
    * QR:QR二维码，数值为0
    * EAN13:EAN条形码标准版，数值为1
    * EAN8:EAN条形码简版，数值为2
    * AZTEC:Aztec二维码，数值为3
    * DATAMATRIX: Data Matrix二维码，数值为4
    * UPCA: UPC条形码标准版，数值为5
    * UPCE: UPC条形码缩短版，数值为6
    * CODEBAR:Codebar条形码，数值为7
    * CODE39:code39条形码，数值为8
    * CODE93: Code93条形码，数值为9
    * CODE128: Code128条形码，数值为10
    * ITF: ITF条形码，数值为11
    * MAXICODE: MaxiCode二维码，数值为12
    * PDF417: PDF 417二维条码，数值为13
    * RSS14: RSS 14条形组合码，数值为14
    * RSSEXPANDED: 扩展式RSS条形组合码，数值为15
    
  方法：
    * scan:扫描识别图片中的条码
    * create:创建扫码识别控件对象
    * getBarcodeById:查找扫码识别控件对象
  对象
    * Barcode:扫码识别控件对象
    * BarcodeStyles:Barcode扫码控件样式
    * BarcodeOptions:条码识别控件扫描参数
  回调方法：
    * BarcodeSuccessCallback: 扫码识别成功回调函数
    * BarcodeErrorCallback: 扫码识别错误回调函数

***
### scan:扫码识别图中的条码
`plus.barcode.scan(path,successCB,errorCB,filters)`
说明：输入图片文件进行扫码识别，成功扫描到条码后通过successCallback回调返回，失败则通过erroeCallback回调返回
参数：
  * path:(string)要扫码的图片路径，必须是本地文件路径，如URLType类型（如以"_www"、"_doc"、"_documents"、"_downloads"开头的相对URL路径）或者系统的绝对路径
  * successCB: ( BarcodeSuccessCallback ) 必选 扫码识别成功回调函数，成功识别到条码（一维码或二维码）时触发，回调函数中返回码类型及码数据
  * errorCB: ( BarcodeErrorCallback ) 可选 扫码识别失败回调函数，扫码识别中发生错误时触发，回调函数中返回错误码及错误描述信息。
  * filters: ( Array ) 可选 条码类型过滤器,条码类型常量数组，默认情况支持QR、EAN13、EAN8类型。 通过此参数可设置扫码识别支持的条码类型（注意：设置支持的条码类型越多，扫描识别速度可能将会降低）
  
***
### create:创建扫码识别对象
`plus.barcode.create(id,filters,styles)`
说明：
  此方法创建扫码识别控件并不会显示在页面中，需要调用plus.webview.Webview窗口对象的append方法将其添加到Webview窗口中才能显示。注意：需要设置styles参数的top/left/width/height属性指定扫码识别控件的位置及大小，否则无法正常显示，
参数：
   * id:(S)扫码识别控件标识，可用于通过plus.barcode.getBarcodeById()方法查找已经创建的扫码识别控件对象
   * filters:(Array/Number)条码类型过滤器，条码类型数组，默认情况下支持QR、EAN13、EAN8类型。通过此参数可设置扫码识别支持的条码类型
   * styles:(BarcodeStyles)扫码识别控件样式，用于设置扫码控件在页面中的显示样式，如扫码框，颜色等

