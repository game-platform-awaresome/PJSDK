#泡椒网游戏联运SDK 问题反馈：<pengjianbo@paojiao.cn>**附加：如果你的项目是使用gradle构建的，使用就非常简单了。搭建的步骤就不需要了，直接使用gradle:**```groovycompile 'com.paojiao.sdk:3.2'```#搭建1、拷贝PJSDK-Final-xxx.jar到你的工程libs目录下2、拷贝res下的资源拷到你的工程相应目下3、配置PJSDK所需的权限 ```xml<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" /><uses-permission android:name="android.permission.INTERNET"/><!--在SDCard中创建与删除文件权限  --><uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/><!-- 往SDCard写入数据权限 --><uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/><!-- 从SDCard读取数据权限 --><uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/><uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/><uses-permission android:name="android.permission.SYSTEM_OVERLAY_WINDOW" /><uses-permission android:name="android.permission.READ_PHONE_STATE"/>```4、配置Acitivity和Service```xml <service android:name="com.paojiao.sdk.service.FloatViewService"     android:exported="false"/> <activity android:name="com.paojiao.sdk.H5WebViewActivity"/> <activity android:name="com.paojiao.sdk.CustomerServicesActivity"/>```#使用1、初始化```javaPJSDK.initialize(getBaseContext(), APP_ID, APP_KEY, true);```2、登录注册```javaPJSDK.doLogin(new LoginListener() {    @Override     public void onSuccess(UserBean user) {        super.onSuccess(user);    }    @Override     public void onFailure() {        super.onFailure();    }    });```3、支付```java// 订单标题，如：购买100元宝String subject = "购买1个元宝";// 订单价格，单位RMB元，浮点类型float price = 0.1f;// 合作方自定义参数，一般为订单号String ext = "NO123456789";// 该订单的备注信息String remark = "订单备注信息";PJSDK.doPay(subject, price, remark, ext, new PayListener() {    @Override    public void onPaySuccess() {        Toast.makeText(MainActivity.this, "pay success", Toast.LENGTH_SHORT).show();    }    @Override    public void onPayFailure() {    Toast.makeText(MainActivity.this, "pay failure ", Toast.LENGTH_SHORT).show();    }    @Override    public void onPayCancel() {        Toast.makeText(MainActivity.this, "pay cancel", Toast.LENGTH_SHORT).show();    }});```4、FloatView控制```javaPJSDK.showFloatingView();//显示悬浮LOGOPJSDK.hideFloatingView();//隐藏悬浮LOGO``` 5、提交玩家信息  ```java RoleInfo roleInfo = new RoleInfo("胡哥哥", 69, "才高八斗", 7554);PJSDK.uploadPlayerInfo(roleInfo, new HttpListener() {    }); ``` #SDK升级 PJSDK这个版本全新的代码，请把SDK 3.1以前的资源全部移除再使用#混淆```properties#########################通用混淆配置，项目中有就不需要配置#####################-keep public class * extends android.app.Activity-keep public class * extends android.app.Service-keep public class * extends android.content.BroadcastReceiver-keepclasseswithmembers class * {public <init>(android.content.Context, android.util.AttributeSet);}-keepclasseswithmembers class * {public <init>(android.content.Context, android.util.AttributeSet, int);}-keepclassmembers class * extends android.app.Activity {public void *(android.view.View);}#########################PJSDK混淆配置#####################-keep class com.paojiao.sdk.*{*;}-keepclassmembers class com.paojiao.sdk.H5WebViewActivity$PJJavascriptInterface {  public *;}-keepattributes *Annotation*-keepattributes *JavascriptInterface*``` #常见问题及意见1、屏幕旋转对话框消失问题	遇到这个问题请参考demo中manifest activity configChange配置（android:configChanges="orientation|keyboardHidden|screenSize"）2、如果游戏出现crash后而游戏并没退出应该再次对SDK再次初始化3、用户按Home键隐藏泡椒悬浮LOGO,请开发商自行处理，我们提供了显示（PJSDK.showFloatingView();）和隐藏（PJSDK.hideFloatingView();）方法4、配置SDK H5WebViewActivity时根据游戏配置H5WebViewActivity的屏幕方向（建议配置）