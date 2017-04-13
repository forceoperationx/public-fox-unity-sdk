[TOP](../../README.md)　>　**최신 버전에의 Migration**

---

# 최신 버전에의 Migration에 대하여

이전의 F.O.X SDK가 도입된 앱에 최신의 SDK를 도입할 시에 필요한 절차는 아래와 같습니다.

* [1. 이전의 버전의 파일을 전부 삭제](#remove_regacy)
* [2. 최신 버전의 파일을 프로젝트에 인스톨](#install_plugins)
* [3. 구 버전(4.0.0미만)부터의 실장 방법을 갱신](#update_implementation)
* [4. 4.0.0 에서  4.0.1 으로의 업데이트](#update_401)
* [5. 기타](#other)

<div id="remove_regacy"></div>

## 1. 이전의 버전의 파일을 모두 삭제

#### 1.1 Unity플러그인 파일 리스트

|삭제 대상 파일명|대상OS|비고|
|:---|:---:|:---|
|FoxPlugin.cs|공통||
|FoxPluginDefaults.cs|공통||
|FoxAnalyticsSession.cs|공통||
|FoxPluginAndroid.cs|Android||
|FoxPluginIOS.cs|iOS||
|[Trade]|공통|광고 배급용 라이브러리 디렉토리<br>※도입되어 있는 않은 경우에는 삭제 대상외입니다.|
|　Api/AdPosition.cs|공통||
|　Api/DahliaBannerAds.cs|공통||
|　Api/DahliaInterstitialAds.cs|공통||
|　Common/DummyClient.cs|공통||
|　Common/IAdListener.cs|공통||
|　Common/IDahliaAdsBannerClient.cs|공통||
|　Common/IDahliaAdsInterstitialClient.cs|공통||
|　Platforms/DahliaAdsClientFactory.cs|공통||
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
|Scripts/FOX/FoxPluginJS.js|공통||

#### 1.2 Android프로젝트의 삭제 대상 파일

**[삭제]**

* 프로젝트내의 libs 디렉토리의 FOX_Android_SDK_&lt;VERSION&gt;.jar
* AndroidManifest.xml에 기술되어 있는 이하의 meta-data(금후 코드상의 설정이 됩니다.)

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

#### 1.3 iOS XCode프로젝트의 삭제 대상 파일

1.1의 리스트 이외에 이하의 ２파일도 삭제합니다.

**[삭제]**

* libAppAdForce.a
* AppAdForce.plist


<div id="install_plugins"></div>

## 2. 최신 버전의 파일을 프로젝트에 인스톨

> [Unity 플러그인의 도입 방법](./doc/integration/README.md)를 확인하여 주십시오.

<div id="update_implementation"></div>

## 3. 구 버전(4.0.0미만)부터의 실장 방법을 갱신

|종별|`〜 3.3.0` 의 실장|`4.0.0 〜` 의 실장|
|:---:|:---|:---|
|[액티베이션](../../README.md#activate_sdk)|**[iOS]**<br>・AppAdForce.plist<br><br>**[Android]**<br>AndroidManifest.xml<br><br>・APPADFORCE_APP_ID<br>・APPADFORCE_CRYPTO_SALT<br>・ANALYTICS_APP_KEY|コード上にて<br><br>FoxConfig config = new FoxConfig ();<br>config.iOSAppId = 발행된 iOS용 APP_ID;<br>config.iOSAppKey = 발행된 iOS용 APP_KEY;<br>config.iOSAppSalt = 발행된 iOS용 APP_SALT;<br>config.androidAppId = 발행된 Android용 APP_ID;<br>config.androidAppKey = 발행된 Android용 APP_KEY;<br>config.androidAppSalt = 발행된 Android용 APP_SALT;<br>Fox.activate(config);|
|[인스톨 계측](../../README.md#track_install)|FoxPlugin.sendConversion("default");|Fox.trackInstall();|
|[인스톨 계측<br>옵션 부가](../track_install/README.md)|FoxPlugin.sendConversion("http://yourhost.com", "USER_001");|FoxTrackOption option = new FoxTrackOption();<br>option.redirectURL = "http://yourhost.com";<br>option.buid = "USER_001";<br>Fox.sendConversion(option);|
|외부 브라우저로 이벤트 계측|-|Fox.trackEventByBrowser("http://yourhost.com");|
|[세션 계측](../../README.md#track_session)|FoxPlugin.sendStartSession();|Fox.trackSession();|
|[이벤트 계측 1](../track_event/README.md#add_buid)|// 튜토리얼 완료 이벤트<br>int ltvId = 성과 지점ID;<br>FoxPlugin.sendLtv(ltvId, "USER_001");<br>FoxPlugin.sendEvent("_tutorial_comp", null, null, 0);|// 튜토리얼 완료 이벤트<br>int ltvId = 성과 지점ID;<br>FoxEvent e = new FoxEvent("_tutorial_comp", ltvId);<br>	e.buid = "USER_001";<br>Fox.trackEvent(e);|
|[이벤트 계측 2](../track_event/README.md#purchase)|// LTV계측에 의한 과금 계측<br>FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");<br>FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "1.2");<br>FoxPlugin.sendLtv(성과 지점 ID);<br><br>// 액세스 분석에 의한 과금 계측<br>FoxPlugin.sendEventPurchase("_purchase", null, null, "ABCDEFG12345", "A-001", "Coin", 1.2, 1, "USD");|//과금 이벤트 계측<br>FoxEvent purchase = FoxEvent.makePurchase("_purchase", 성과 지점 ID, 1.2, "USD");<br>purchase.buid = "USER_001";<br>purchase.orderId = "ABCDEFG12345";<br>purchase.itemName = "Coin";<br>purchase.sku = "A-001"<br>purchase.quantity = 1;<br>Fox.trackEvent(purchase);|

> ※1 버전 4.0.0이후에 Migrationg하는 경우 이제까지 구 버전에서 지정하고 있던 이벤트명을 변경하는 경우 액세스 분석에서 계측하고 있는 집계 데이터가 연관없어지므로 주의하여 주십시오.

<div id="update_401"></div>

## 4. 4.0.0 부터 4.0.1 에의 업데이트

#### (iOS) 파일의 교체

버전4.0.1을 인스톨할 시 아래의 파일을 교체 하여 주십시오

|대응|대상 파일|
|:---:|:---:|
|삭제|CYZUFoxReengagePlugin.h|
|삭제|CYZUFoxReengagePlugin.m|
|추가|CYZFoxAppDelegateSwizzling.m|


<div id="other"></div>

## 5. 기타

#### (Android) BroadcastReceiver의 복수 지정

**[`〜 3.3.0` 의 실장]**

```xml
<!-- 레시버 클래스는 F.O.X SDK의 클래스를 지정 -->
<receiver
    android:name="jp.appAdForce.android.InstallReceiver"
    android:exported="true">
    <intent-filter>
        <action android:name="com.android.vending.INSTALL_REFERRER" />
    </intent-filter>
</receiver>

<!-- F.O.X SDK에서 호출하고픈 다른 레시버 클래스 정보를 meta-data로 기술 -->
<meta-data
        android:name="APPADFORCE_FORWARD_RECEIVER"
        android:value="com.example.InstallReceiver" />
```

**[[`4.0.0 〜` 의 실장](../integration/android/install_referrer/README.md)]**

```xml
<!-- 레시버 클래스는 F.O.X SDK의 클래스를 지정합니다. -->
<receiver
    android:name="co.cyberz.fox.FoxInstallReceiver"
    android:exported="true">
    <intent-filter>
        <action android:name="com.android.vending.INSTALL_REFERRER" />
    </intent-filter>
</receiver>

<!-- F.O.X SDK에서 호출하고픈 다른 레시버 클래스의 패스를 |(파이프)로 구별하여 기술합니다. -->
<meta-data
        android:name="APPADFORCE_FORWARD_RECEIVER"
        android:value="com.example.InstallReceiver1|com.example.InstallReceiver2|com.example.InstallReceiver3" />
```

---
[톱](/lang/ja/README.md)
