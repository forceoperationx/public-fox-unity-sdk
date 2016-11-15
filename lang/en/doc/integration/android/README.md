# Setting up Android Project

To setup the SDK for android, you need to edit the AndroidManifest.xml file in the Unity project. Rename "Plugins/Android/AndroidManifest-sample.xml" to "AndroidManifest.xml" in case AndroidManifest.xml file does not exist in your project.

## Setting up the permissions

FOX sdk uses the following four permssion on android.
 In &lt;Manifest&gt; tag, add the following permission.

```xml:
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

Permission|Protection Level|Requirement|Summary
:---|:---:|:---:|:---
INTERNET|Normal|Required|Required to communicate to F.O.X servers.
ACCESS_NETWORK_STATE|Normal|Required|Required to check if communication with servers is possible.
READ_EXTERNAL_STORAGE|Dangerous|Can be skipped|Required to perform duplicate entry prevention checks.(※1)
WRITE_EXTERNAL_STORAGE|Dangerous|Can be skipped|Required to perform duplicate entry prevention checks.(※1)

> ※1 In Android M onwards, to do something that requires a `dangerous` ProtectionLevel permission, user's permission is necessary. See [Duplicate entry prevention using external storage](/lang/en/doc/integration/android/external_storage/README.md) for details.

## Setting up the metadata

Add the following metadata tags containing the information to be used by F.O.X SDK.

```xml
<meta-data
	android:name="APPADFORCE_APP_ID"
	android:value="Will be informed to you by a Force Operation X representative." />
<meta-data
	android:name="APPADFORCE_SERVER_URL"
	android:value="Will be informed to you by a Force Operation X representative." />
<meta-data
	android:name="APPADFORCE_CRYPTO_SALT"
	android:value="Will be informed to you by a Force Operation X representative." />
<meta-data
	android:name="ANALYTICS_APP_KEY"
	android:value="Will be informed to you by a Force Operation X representative." />
```

Here are the key-value pairs included in the metadata above.

|Parameter|Requirement|Summary|
|:------|:------|:------|
|APPADFORCE_APP_ID|Required|Will be informed to you by a Force Operation X representative.|
|APPADFORCE_SERVER_URL|Required|Will be informed to you by a Force Operation X representative.|
|APPADFORCE_CRYPTO_SALT|Required|Will be informed to you by a Force Operation X representative.|
|ANALYTICS_APP_KEY|Required|Will be informed to you by a Force Operation X representative.|


## Setting up the Install referrer
To perform measurement using an install referrer, add the following tags inside the &gt;application&gt; tag.

```xml
<receiver android:name="jp.appAdForce.android.InstallReceiver" android:exported="true">
	<intent-filter>
		<action android:name="com.android.vending.INSTALL_REFERRER" />
	</intent-filter>
</receiver>
```

In case there already exists a receiver class for "com.android.vending.INSTALL_REFERRER", refer to [Making two INSTALL_REFERRER receivers coexist](/lang/en/doc/integration/android/install_referrer/README.md).

## Setting up re-engagement tracking

Add the necessary settings to track app launches via custom URL scheme in the &lt;application&gt; tag. The `IntentReceiverActivity` mentioned below is an Activity provided with the F.O.X SDK.

Re-engagement tracking is performed by the `IntentReceiverActivity` that will be called by the custom URL scheme. Make sure you specify a URL scheme that is not used to launch any other activities.

```xml
<activity android:name="jp.appAdForce.android.IntentReceiverActivity">
	<intent-filter>
		<action android:name="android.intent.action.VIEW" />
		<category android:name="android.intent.category.DEFAULT" />
		<category android:name="android.intent.category.BROWSABLE" />
		<data android:scheme="custom URL scheme" />
	</intent-filter>
</activity>
```

## Other

* [The implementation of Google Play Services SDK to use advertisement ID](/lang/en/doc/integration/android/google_play_services/README.md)

* [Sample AndroidManifest.xml](/lang/en/doc/integration/android/config_android_manifest/AndroidManifest.xml)

* [（Optional）Duplicate entry prevention using external storage](/lang/en/doc/integration/android/external_storage/README.md)

* [（Option）Using the Android M auto backup feature](/lang/en/doc/integration/android/auto_backup/README.md)


## If using ProGuard

When obfuscating the app source code using ProGuard, please add the following settings so as to remove F.O.X SDK from target files.

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

Also, in case of importing Google Play Services SDK, please check if keep specification of ProGuard is included from page below.

[Setting Up Google Play Services](https://developer.android.com/google/play-services/setup.html#Proguard)


---
[PREVIOUS](/lang/en/doc/integration/README.md)

[TOP](/lang/en/README.md)
