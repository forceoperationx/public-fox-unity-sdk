# Android項目的設定

能夠在Unity項目裡進行Android開發設定。編輯項目中的AndroidManifest.xml。如果不存在AndroidManifest.xml文件，請把「Plugins/Android/AndroidManifest-sample.xml」文件名變成「AndroidManifest.xml」來使用。

## permission的設定

F.O.X SDK利用下面4種permission。
請在&lt;Manifest&gt;tag內添加如下permission設定。

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

permission|Protection Level|必須|概要
:---|:---:|:---:|:---
INTERNET|Normal|必須|F.O.X SDK進行通信時必須。
ACCESS_NETWORK_STATE|Normal|必須|F.O.X SDK確認可否通信時必須。
READ_EXTERNAL_STORAGE ※1|Dangerous|任意|利用外部存儲提高重複排重功能時必須。(※2)
WRITE_EXTERNAL_STORAGE ※1|Dangerous|任意|利用外部存儲提高重複排重功能時必須。(※2)

> ※1 READ_EXTERNAL_STORAGE和WRITE_EXTERNAL_STORAGE權限是利用把數據保存在外部存儲器，為了再安裝APP的時候更準確地進行Install計測而必須設定的權限，屬於任意設定。

> ※2 Android M為了利用ProtectionLevel設定為`dangerous`權限的機能，需要獲得用戶許可。詳細請查看[利用外部存儲設定重複排除](/lang/zh-tw/doc/integration/android/external_storage/README.md)

## meta-data的設定

為了執行SDK，請將必要的訊息添加到&lt;application&gt;tag內。

```xml
<meta-data
	android:name="APPADFORCE_APP_ID"
	android:value="請輸入Force Operation X管理員告知的值。" />
<meta-data
	android:name="APPADFORCE_SERVER_URL"
	android:value="請輸入Force Operation X管理員告知的值。" />
<meta-data
	android:name="APPADFORCE_CRYPTO_SALT"
	android:value="請輸入Force Operation X管理員告知的值。" />
<meta-data
	android:name="ANALYTICS_APP_KEY"
	android:value="請輸入Force Operation X管理員告知的值。" />
<meta-data
	android:name="ANALYTICS_SERVER_URL"
	android:value="請輸入Force Operation X管理員告知的值。" />
```

設定的Key和Value如下：

|參數名|必須|概要|
|:------|:------|:------|
|APPADFORCE_APP_ID|必須|請輸入Force Operation X管理員告知的值。|
|APPADFORCE_SERVER_URL|必須|請輸入Force Operation X管理員告知的值。|
|APPADFORCE_CRYPTO_SALT|必須|請輸入Force Operation X管理員告知的值。|
|ANALYTICS_APP_KEY|必須|請輸入Force Operation X管理員告知的值。|
|ANALYTICS_SERVER_URL|必須|請輸入Force Operation X管理員告知的值。|

## Install referrer計測的設定
將Install referrer計測所需要的設定添加在&lt;application&gt;tag內。

```xml
<receiver android:name="jp.appAdForce.android.InstallReceiver" android:exported="true">
	<intent-filter>
		<action android:name="com.android.vending.INSTALL_REFERRER" />
	</intent-filter>
</receiver>
```

如果"com.android.vending.INSTALL_REFERRER"的receiver class已經被定義，請參照[讓多個INSTALL_REFERRER seceiver共存的設定](/lang/zh-tw/doc/integration/android/install_referrer/README.md)

## Reengagement計測的設定

為了進行Reengagement廣告的計測（經由定製URL Scheme啟動的計測），需在&lt;application&gt;標簽裡添加必要的設定。
下面的`IntentReceiverActivity`是利用F.O.X SDK提供的Activity。

Reengagement計測是利用定製URLScheme來調用Activity的方式進行計測。
這個定製的URL Scheme，請區別開其他Activity裡的值來設定。

```xml
<activity android:name="jp.appAdForce.android.IntentReceiverActivity">
	<intent-filter>
		<action android:name="android.intent.action.VIEW" />
		<category android:name="android.intent.category.DEFAULT" />
		<category android:name="android.intent.category.BROWSABLE" />
		<data android:scheme="定製的URL Scheme" />
	</intent-filter>
</activity>
```

## 其他

* [導入Google Play Services SDK來使用廣告ID](/lang/zh-tw/doc/integration/android/google_play_services/README.md)

* [AndroidManifest.xml設定範例](/lang/zh-tw/doc/integration/android/config_android_manifest/AndroidManifest.xml)

* [（任意）利用外部存儲設定重複排除](/lang/zh-tw/doc/integration/android/external_storage/README.md)

* [（任意）Android M(6.0) 利用自動備份功能](/lang/zh-tw/doc/integration/android/auto_backup/README.md)

## 使用ProGuard

如果沒有使用ProGuard功能，可以跳過本節。
使用ProGuard進行APP讀取混淆化時，請進行以下設定，將F.O.X SDK的method排除在對象外。

```
-keepattributes *Annotation*

-libraryjars libs/AppAdForce.jar
-keep interface jp.appAdForce.** { *; }
-keep class jp.appAdForce.** { *; }
-keep class jp.co.dimage.** { *; }
-keep class com.google.android.gms.ads.identifier.* { *; }
-dontwarn jp.appAdForce.android.**
-dontwarn jp.co.dimage.**
-dontwarn jp.co.cyberz.fox.**
-dontwarn com.adobe.fre.**
-dontwarn com.ansca.**
-dontwarn com.naef.jnlua.**
```

---
[返回](/lang/zh-tw/doc/integration/README.md)

[TOP](/lang/zh-tw/README.md)
