# Setting of Android Project

The setting for Android can be conducted on Unity project. Edit AndroidManifest.xml which is put into Unity project. In the case that AndroidManifest.xml does not exist in project, use after renaming from 「Plugins/Android/AndroidManifest-sample.xml」to「AndroidManifest.xml」.


## Setting of permission

In F.O.X SDK, use the 4 following permissions.
 In &lt;Manifest&gt; tag, add the setting of next permissions.

```xml:
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

Permission|Protection Level|Mandatory|Outline
:---|:---:|:---:|:---
INTERNET|Normal|Mandatory|It is necessary so that F.O.X SDK operates communication.
ACCESS_NETWORK_STATE|Normal|Mandatory|It is necessary so that F.O.X SDK operates communication.
READ_EXTERNAL_STORAGE|Dangerous|Arbitrary|It is necessary to improve deduplication using storage.(※1)
WRITE_EXTERNAL_STORAGE|Dangerous|Arbitrary|It is necessary to improve deduplication using storage.(※1)

> ※1 To use the function which is necessary to use the permission specified that ProtectionLevel is dangerous by Android M, it is necessary to get the permission from users. For more detail, check Setting of deduplication using external storage.


## Setting of metadata

Add the necessary information for conducting SDK in &lt;Application&gt;tag.

```xml
<meta-data
	android:name="APPADFORCE_APP_ID"
	android:value="There will be a contact from the administrator of Force Operation X, so please type the value." />
<meta-data
	android:name="APPADFORCE_CRYPTO_SALT"
	android:value="There will be a contact from the administrator of Force Operation X, so please type the value." />
<meta-data
	android:name="ANALYTICS_APP_KEY"
	android:value="There will be a contact from the administrator of Force Operation X, so please type the value." />
```

Key and value for setting are following.

|Name of parameter|Mandatory|Outline|
|:------|:------|:------|
|APPADFORCE_APP_ID|Mandatory|There will be a contact from the administrator of Force Operation X, so please type the value.|
|APPADFORCE_CRYPTO_SALT|Mandatory|There will be a contact from the administrator of Force Operation X, so please type the value.|
|ANALYTICS_APP_KEY|Mandatory|There will be a contact from the administrator of Force Operation X, so please type the value.|


## Setting of Install referrer
To conduct installation measurement using install_referrer, add the following setting into &lt;application&gt;tag

```xml
<receiver android:name="jp.appAdForce.android.InstallReceiver" android:exported="true">
	<intent-filter>
		<action android:name="com.android.vending.INSTALL_REFERRER" />
	</intent-filter>
</receiver>
```

In the case that receiver class to "com.android.vending.INSTALL_REFERRER" is already defined, please refer to Setting in [the case of making two INSTALL_REFERRER receivers coexist](/lang/en/doc/integration/android/install_referrer/README.md).

## Setting of re-engagement measurement

Add the necessary setting for measure the startings via custom URL scheme in &lt;application&gt;tag. The following IntntReceiverActivity is the Activity that is provided with SDK.

Re-engagement meaurement is performed by calling this Antivity with custom URL scheme. <br>
Use different value for this custom URL scheme from one set in other Activit.

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

* [Setting sample for AndroidManifest.xml](/lang/en/doc/integration/android/config_android_manifest/AndroidManifest.xml)

* [（Option）Setting og deduplication using external storages](/lang/en/doc/integration/android/external_storage/README.md)

* [（Option）Use of Android M auto backup funtion](/lang/en/doc/integration/android/auto_backup/README.md)


## In the case of using ProGuard

When obfuscating application using ProGuard, please add the following setting not to being subject to method of F.O.X SDK.

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

Also, in the case of implementing Google Play Service SDK , please check whether keep specification which is noted in the following page is noted, or not.

[ProGuard support when in the implementation of Google Play Services](https://developer.android.com/google/play-services/setup.html#Proguard)


---
[PREVIOUS](/lang/en/doc/integration/README.md)

[TOP](/lang/en/README.md)
