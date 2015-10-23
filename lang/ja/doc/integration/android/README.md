# Androidプロジェクトの設定

Android 用の設定は Unity プロジェクト上で行うことができます。Unity プロジェクトに組み込まれた
AndroidManifest.xml を編集します。プロジェクトに AndroidManifest.xml が存在しない場合は、 「Plugins/Android/AndroidManifest-sample.xml」を「AndroidManifest.xml」にリネームしてご利用ください。


## パーミッションの設定

<Manifest>タグ内に次のパーミッションの設定を追加します。

```xml:
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

## メタデータの設定

SDKの実行に必要な情報を<application>タグ内に追加します。

```xml:
<meta-data android:name="APPADFORCE_APP_ID" android:value="Force Operation X管理者より連絡しますので、その値を入力してください。" />
<meta-data android:name="APPADFORCE_SERVER_URL" android:value="Force Operation X管理者より連絡しますので、その値を入力してください。" />
<meta-data android:name="APPADFORCE_CRYPTO_SALT" android:value="Force Operation X管理者より連絡しますので、その値を入力してください。" />
<meta-data android:name="ANALYTICS_APP_KEY" android:value="Force Operation X管理者より連絡しますので、その値を入力してください。" />
```

設定するキーとバリューは以下の通りです。

|パラメータ名|必須|概要|
|:------|:------|:------|
|APPADFORCE_APP_ID|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|
|APPADFORCE_SERVER_URL|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|
|APPADFORCE_CRYPTO_SALT|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|
|ANALYTICS_APP_KEY|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|


## インストールリファラ計測の設定
インストールリファラーを用いたインストール計測を行うために下記の設定を<application>タグに追加します。

```xml:
<receiver android:name="jp.appAdForce.android.InstallReceiver" android:exported="true">
	<intent-filter>
		<action android:name="com.android.vending.INSTALL_REFERRER" />
	</intent-filter>
</receiver>
```

既に"com.android.vending.INSTALL_REFERRER"に対するレシーバークラスが定義されている場合には、[二つのINSTALL_REFERRERレシーバーを共存させる場合の設定](https://github.com/cyber-z/public_fox_android_sdk/blob/master/doc/install_referrer/ja/README.md)をご参照ください。


## リエンゲージメント計測の実装

カスタムURLスキーム経由の起動を計測するために必要な設定を<application>タグ内に追記します。
カスタムURLスキームは他のActivityで設定しているものと異なる値を設定してください。

```xml
<activity android:name="jp.appAdForce.android.IntentReceiverActivity">
	<intent-filter>
		<action android:name="android.intent.action.VIEW" />
		<category android:name="android.intent.category.DEFAULT" />
		<category android:name="android.intent.category.BROWSABLE" />
		<data android:scheme="カスタム URL スキーム" />
	</intent-filter>
</activity>
```

[広告IDを利用するためのGoogle Play Services SDKの導入](/lang/ja/doc/google_play_services/)

[（オプション）外部ストレージを利用した重複排除設定](https://github.com/cyber-z/public_fox_android_sdk/tree/master/doc/external_storage/ja/)

[AndroidManifest.xmlサンプル](/lang/ja/doc/config_android_manifest/AndroidManifest.xml)


## ProGuardを利用する場合

ProGuard を利用してアプリケーションの難読化を行う際は F.O.X SDK のメソッドが対象とならないよう、以下の設定 を追加してください。

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
-dontwarn com.adobe.fre.FREContext
-dontwarn com.adobe.fre.FREExtension
-dontwarn com.adobe.fre.FREFunction
-dontwarn com.adobe.fre.FREObject
-dontwarn com.ansca.**
-dontwarn com.naef.jnlua.**
```

また、Google Play Service SDK を導入されている場合は、以下のぺージに記載されている keep 指定が記述されているかご確認ください。

[Google Play Services導入時のProguard対応](https://developer.android.com/google/play-services/setup.html#Proguard)
