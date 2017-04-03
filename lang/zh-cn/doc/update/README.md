[TOP](../../README.md)　>　**更新到最新版本**

---

# 关于更新到最新版本

以前导入过旧版F.O.X SDK的APP中导入最新SDK时，请按照以下的顺序进行。

* [1. 删除旧版本所有文件](#remove_regacy)
* [2. 在项目中导入最新版文件](#install_plugins)
* [3. 从旧版本(4.0.0以下)更新升级的执行方法](#update_implementation)
* [4. 其他](#other)

<div id="remove_regacy"></div>
## 1. 删除旧版本所有文件

#### 1.1 Cocos2d-x插件文件一览

|删除对象文件名称|对象OS|备注|
|:---|:---:|:---|
|FoxPlugin.cs|共通||
|FoxPluginDefaults.cs|共通||
|FoxAnalyticsSession.cs|共通||
|FoxPluginAndroid.cs|Android||
|FoxPluginIOS.cs|iOS||
|[Trade]|共通|广告投放类库目录<br>※未导入时不删除。|
|　Api/AdPosition.cs|共通||
|　Api/DahliaBannerAds.cs|共通||
|　Api/DahliaInterstitialAds.cs|共通||
|　Common/DummyClient.cs|共通||
|　Common/IAdListener.cs|共通||
|　Common/IDahliaAdsBannerClient.cs|共通||
|　Common/IDahliaAdsInterstitialClient.cs|共通||
|　Platforms/DahliaAdsClientFactory.cs|共通||
|　Platforms/Android/AndroidBannerClient.cs|Android||
|　Platforms/Android/AndroidInterstitialClient.cs|Android||
|　Platforms/Android/DahliaBannerListener.cs|Android||
|　Platforms/Android/DahliaInterstitialListener.cs|Android||
|　Platforms/iOS/Externs.cs|iOS||
|　Platforms/iOS/IOSBannerClient.cs|iOS||
|　Platforms/iOS/IOSInterstitialClient.cs|iOS||
|　Platforms/iOS/MonoPinvokeCallBackAttribute.cs|iOS||
|Plugins/Android/AddAdForce_&lt;version&gt;|Android||
|Plugins/iOS/AdManager.h|iOS||
|Plugins/iOS/AnalyticsManager.h|iOS||
|Plugins/iOS/DLAdStateDelegate.h|iOS||
|Plugins/iOS/DLBannerView.h|iOS||
|Plugins/iOS/DLInterstitialViewController.h|iOS||
|Plugins/iOS/DLUAdStateDelegateImp.h|iOS||
|Plugins/iOS/DLUAdStateDelegateImp.m|iOS||
|Plugins/iOS/DLUInterface.h|iOS||
|Plugins/iOS/DLUInterface.m|iOS||
|Plugins/iOS/FoxNotifyPlugin.h|iOS||
|Plugins/iOS/FoxNotifyPlugin.m|iOS||
|Plugins/iOS/FoxReengagePlugin.h|iOS||
|Plugins/iOS/FoxReengagePlugin.m|iOS||
|Plugins/iOS/FoxVersionPlugin.h|iOS||
|Plugins/iOS/FoxVersionPlugin.mm|iOS||
|Plugins/iOS/libFoxSdk.h|iOS||
|Plugins/iOS/libFoxSdk.h|iOS||
|Plugins/iOS/libFoxSdk.m|iOS||
|Plugins/iOS/Ltv.h|iOS||
|Plugins/iOS/Notify.h|iOS||
|Scripts/FOX/FoxPluginJS.js|共通||

#### 1.2 Android项目的删除对象文件

**[删除]**

* 项目内libs目录的FOX_Android_SDK_&lt;VERSION&gt;.jar
* AndroidManifest.xml中记述的下列meta-data(之后在代码上设置)

```xml
<meta-data
    android:name="APPADFORCE_APP_ID"
    android:value="1234" />
<meta-data
    android:name="APPADFORCE_SERVER_URL"
    android:value="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" />
<meta-data
    android:name="APPADFORCE_CRYPTO_SALT"
    android:value="YYYYYYYYYYYYYYYYYYYYYYYYYY" />
<meta-data
    android:name="ANALYTICS_APP_KEY"
    android:value="ZZZZZZZZZZZZZZZZZZZZZZZZZZ" />
```

#### 1.3 iOS XCode项目的删除对象文件

1.1的一览文件以外，还要删除以下两项文件。

**[删除]**

* libAppAdForce.a
* AppAdForce.plist


<div id="install_plugins"></div>
## 2. 在项目中导入最新版文件

> 请确认[Unity插件的导入方法](./doc/integration/README.md)。

<div id="update_implementation"></div>
## 3. 从旧版本(4.0.0以下)更新升级的执行方法

|分类|`〜 3.3.0` 的执行|`4.0.0 〜` 的执行|
|:---:|:---|:---|
|[激活](../../README.md#activate_sdk)|**[iOS]**<br>・AppAdForce.plist<br><br>**[Android]**<br>AndroidManifest.xml<br><br>・APPADFORCE_APP_ID<br>・APPADFORCE_CRYPTO_SALT<br>・ANALYTICS_APP_KEY|代码上<br><br>FoxConfig config = new FoxConfig ();<br>config.iOSAppId = 发行的iOS APP_ID;<br>config.iOSAppKey = 发行的iOS APP_KEY;<br>config.iOSAppSalt = 发行的iOS APP_SALT;<br>config.androidAppId = 发行的Android APP_ID;<br>config.androidAppKey = 发行的Android APP_KEY;<br>config.androidAppSalt = 发行的Android APP_SALT;<br>Fox.activate(config);|
|[Install计测](../../README.md#track_install)|FoxPlugin.sendConversion("default");|Fox.trackInstall();|
|[Install计测<br>使用option](../track_install/README.md)|FoxPlugin.sendConversion("http://yourhost.com", "USER_001");|FoxTrackOption option = new FoxTrackOption();<br>option.redirectURL = "http://yourhost.com";<br>option.buid = "USER_001";<br>Fox.sendConversion(option);|
|通过外部浏览器进行事件计测|-|Fox.trackEventByBrowser("http://yourhost.com");|
|[session计测](../../README.md#track_session)|FoxPlugin.sendStartSession();|Fox.trackSession();|
|[事件计测 1](../track_event/README.md#add_buid)|// 新手引导完成事件<br>int ltvId = 成果地点ID;<br>FoxPlugin.sendLtv(ltvId, "USER_001");<br>FoxPlugin.sendEvent("_tutorial_comp", null, null, 0);|// 新手引导完成事件<br>int ltvId = 成果地点ID;<br>FoxEvent e = new FoxEvent("_tutorial_comp", ltvId);<br>	e.buid = "USER_001";<br>Fox.trackEvent(e);|
|[事件计测 2](../track_event/README.md#purchase)|// 通过LTV计测进行付费计测<br>FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");<br>FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "1.2");<br>FoxPlugin.sendLtv(成果地点 ID);<br><br>// 通过流量分析进行付费计测<br>FoxPlugin.sendEventPurchase("_purchase", null, null, "ABCDEFG12345", "A-001", "Coin", 1.2, 1, "USD");|//付费事件计测<br>FoxEvent purchase = FoxEvent.makePurchase("_purchase", 成果地点 ID, 1.2, "USD");<br>purchase.buid = "USER_001";<br>purchase.orderId = "ABCDEFG12345";<br>purchase.itemName = "Coin";<br>purchase.sku = "A-001"<br>purchase.quantity = 1;<br>Fox.trackEvent(purchase);|

> ※1 请注意，升迁到4.0.0及以上的版本时如果修改旧版本的事件名，将无法保留住到目前为止流量分析中计测到的数据。


<div id="other"></div>
## 4. 其他

#### (Android) BroadcastReceiver多项指定

**[`〜 3.3.0` 的执行]**

```xml
<!-- receiver类指定F.O.X SDK的类 -->
<receiver
    android:name="jp.appAdForce.android.InstallReceiver"
    android:exported="true">
    <intent-filter>
        <action android:name="com.android.vending.INSTALL_REFERRER" />
    </intent-filter>
</receiver>

<!-- 用F.O.X SDK调用其他receiver类的信息用meta-data来描述 -->
<meta-data
        android:name="APPADFORCE_FORWARD_RECEIVER"
        android:value="com.example.InstallReceiver" />
```

**[[`4.0.0 〜` 的执行](../integration/android/install_referrer/README.md)]**

```xml
<!-- receiver类指定F.O.X SDK的类 -->
<receiver
    android:name="co.cyberz.fox.FoxInstallReceiver"
    android:exported="true">
    <intent-filter>
        <action android:name="com.android.vending.INSTALL_REFERRER" />
    </intent-filter>
</receiver>

<!-- 将F.O.X SDK想要调用的其他receiver类的路径用|(分隔符号)区分描述 -->
<meta-data
        android:name="APPADFORCE_FORWARD_RECEIVER"
        android:value="com.example.InstallReceiver1|com.example.InstallReceiver2|com.example.InstallReceiver3" />
```

---
[Top](/lang/zh-cn/README.md)
