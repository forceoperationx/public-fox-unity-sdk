[TOP](../../README.md)　>　**最新バージョンへのマイグレーション**

---

# 最新バージョンへのマイグレーションについて

以前のF.O.X SDKが導入されたアプリに最新のSDKを導入する際に必要な手順は以下の通りです。

* [1. 以前のバージョンのファイルを全て削除](#remove_regacy)
* [2. 最新バージョンのファイルをプロジェクトにインストール](#install_plugins)
* [3. 旧バージョン(4.0.0未満)からの実装方法を更新](#update_implementation)
* [4. その他](#other)

<div id="remove_regacy"></div>
## 1. 以前のバージョンのファイルを全て削除

#### 1.1 Cocos2d-xプラグインファイル一覧

|削除対象ファイル名|対象OS|備考|
|:---|:---:|:---|
|FoxPlugin.cs|共通||
|FoxPluginDefaults.cs|共通||
|FoxAnalyticsSession.cs|共通||
|FoxPluginAndroid.cs|Android||
|FoxPluginIOS.cs|iOS||
|[Trade]|共通|広告配信用ライブラリディレクトリ<br>※導入していなければ削除対象外です。|
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

#### 1.2 Androidプロジェクトの削除対象ファイル

**[削除]**

* プロジェクト内のlibsディレクトリのFOX_Android_SDK_&lt;VERSION&gt;.jar
* AndroidManifest.xmlに記述していた以下のmeta-data(今後、コード上での設定となります)

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

#### 1.3 iOS XCodeプロジェクトの削除対象ファイル

1.1の一覧以外に以下の２ファイルも削除します。

**[削除]**

* libAppAdForce.a
* AppAdForce.plist


<div id="install_plugins"></div>
## 2. 最新バージョンのファイルをプロジェクトにインストール

> [Unityプラグインの導入方法](./doc/integration/README.md)をご確認ください。

<div id="update_implementation"></div>
## 3. 旧バージョン(4.0.0未満)からの実装方法を更新

|種別|`〜 3.3.0` の実装|`4.0.0 〜` の実装|
|:---:|:---|:---|
|[アクティベーション](../../README.md#activate_sdk)|**[iOS]**<br>・AppAdForce.plist<br><br>**[Android]**<br>AndroidManifest.xml<br><br>・APPADFORCE_APP_ID<br>・APPADFORCE_CRYPTO_SALT<br>・ANALYTICS_APP_KEY|コード上にて<br><br>FoxConfig config = new FoxConfig ();<br>config.iOSAppId = 発行されたiOS用APP_ID;<br>config.iOSAppKey = 発行されたiOS用APP_KEY;<br>config.iOSAppSalt = 発行されたiOS用APP_SALT;<br>config.androidAppId = 発行されたAndroid用APP_ID;<br>config.androidAppKey = 発行されたAndroid用APP_KEY;<br>config.androidAppSalt = 発行されたAndroid用APP_SALT;<br>Fox.activate(config);|
|[インストール計測](../../README.md#track_install)|FoxPlugin.sendConversion("default");|Fox.trackInstall();|
|[インストール計測<br>オプション付](../track_install/README.md)|FoxPlugin.sendConversion("http://yourhost.com", "USER_001");|FoxTrackOption option = new FoxTrackOption();<br>option.redirectURL = "http://yourhost.com";<br>option.buid = "USER_001";<br>Fox.sendConversion(option);|
|外部ブラウザでイベント計測|-|Fox.trackEventByBrowser("http://yourhost.com");|
|[セッション計測](../../README.md#track_session)|FoxPlugin.sendStartSession();|Fox.trackSession();|
|[イベント計測 1](../track_event/README.md#add_buid)|// チュートリアル完了イベント<br>int ltvId = 成果地点ID;<br>FoxPlugin.sendLtv(ltvId, "USER_001");<br>FoxPlugin.sendEvent("_tutorial_comp", null, null, 0);|// チュートリアル完了イベント<br>int ltvId = 成果地点ID;<br>FoxEvent e = new FoxEvent("_tutorial_comp", ltvId);<br>	e.buid = "USER_001";<br>Fox.trackEvent(e);|
|[イベント計測 2](../track_event/README.md#purchase)|// LTV計測による課金計測<br>FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");<br>FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "1.2");<br>FoxPlugin.sendLtv(成果地点 ID);<br><br>// アクセス解析による課金計測<br>FoxPlugin.sendEventPurchase("_purchase", null, null, "ABCDEFG12345", "A-001", "Coin", 1.2, 1, "USD");|//課金イベント計測<br>FoxEvent purchase = FoxEvent.makePurchase("_purchase", 成果地点 ID, 1.2, "USD");<br>purchase.buid = "USER_001";<br>purchase.orderId = "ABCDEFG12345";<br>purchase.itemName = "Coin";<br>purchase.sku = "A-001"<br>purchase.quantity = 1;<br>Fox.trackEvent(purchase);|

> ※1 バージョン4.0.0以降にマイグレーションする際、これまで旧バージョンで指定していたイベント名を変更してしまうと、アクセス解析にて計測してきた集計データが引き継がれなくなりますのでご注意ください。


<div id="other"></div>
## 4. その他

#### (Android) BroadcastReceiverの複数指定

**[`〜 3.3.0` の実装]**

```xml
<!-- レシーバークラスはF.O.X SDKのクラスを指定 -->
<receiver
    android:name="jp.appAdForce.android.InstallReceiver"
    android:exported="true">
    <intent-filter>
        <action android:name="com.android.vending.INSTALL_REFERRER" />
    </intent-filter>
</receiver>

<!-- F.O.X SDKから呼び出したい他のレシーバークラス情報をmeta-dataとして記述 -->
<meta-data
        android:name="APPADFORCE_FORWARD_RECEIVER"
        android:value="com.example.InstallReceiver" />
```

**[[`4.0.0 〜` の実装](../integration/android/install_referrer/README.md)]**

```xml
<!-- レシーバークラスはF.O.X SDKのクラスを指定します -->
<receiver
    android:name="co.cyberz.fox.FoxInstallReceiver"
    android:exported="true">
    <intent-filter>
        <action android:name="com.android.vending.INSTALL_REFERRER" />
    </intent-filter>
</receiver>

<!-- F.O.X SDKから呼び出したい他のレシーバークラスのパスを|(パイプ)区切りで記述します -->
<meta-data
        android:name="APPADFORCE_FORWARD_RECEIVER"
        android:value="com.example.InstallReceiver1|com.example.InstallReceiver2|com.example.InstallReceiver3" />
```

---
[トップ](/lang/ja/README.md)
