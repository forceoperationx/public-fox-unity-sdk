[TOP](../../../README.md)　>　[Unity插件导入步骤](../README.md)　>　**Android项目详细设置**

---

# Android项目详细设置*

* [通过Gradle导入](#install_by_gradle)
* [关于AndroidManifest.xml样本](#sample_manifest)
* [权限设置](#permission)
* [install referrer计测设置](#install_referrer)
* [流失唤回广告(Reengagement)计测的执行](#track_reengagement)
* [使用ProGuard](#proguard)
* [其他](#others)

<div id="install_by_gradle"></div>

## 通过Gradle导入

Android Studio项目中使用Gradle导入SDK的情况时，设置以下依存关系(Dependencies)。

```groovy
repositories {
    maven {
        url "https://github.com/cyber-z/public-fox-android-sdk/raw/master/mavenRepo"
    }
}

dependencies {
    compile 'co.cyberz.fox:track-core:4.0.0'
    compile 'co.cyberz.fox.support:track-unity:1.0.0'
}
```


<div id="sample_manifest"></div>

## 关于AndroidManifest.xml样本

可以在Unity项目上进行Android设置。编辑Unity项目中的
AndroidManifest.xml。<br>在项目中不存在AndroidManifest.xml的情况时，将 「Plugins/Android/AndroidManifest-sample.xml」重命名为「AndroidManifest.xml」后来使用。

<div id="permission"></div>

## 权限设置

F.O.X SDK可以使用以下三种权限。
&lt;Manifest&gt;标签中中添加以下权限的设置。

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

权限|Protection Level|必须|概要
:---|:---:|:---:|:---
INTERNET|Normal|必須|F.O.X SDK是进行通信的必要条件。
READ_EXTERNAL_STORAGE|Dangerous|任意|使用外部储存来优化排除重复功能时必须设定。(※1)
WRITE_EXTERNAL_STORAGE|Dangerous|任意|使用外部储存来优化排除重复功能时必须设定。(※1)

> ※1 从Android M开始，使用ProtectionLevel被指定为dangerous权限的功能时，需要用户许可。具体请参考[使用外部储存来优化排除重复功能](./external_storage/README.md)。

<div id="install_referrer"></div>

## install referrer计测设置
使用install referrer进行安装测量时，将以下设置添加至&lt;application&gt;标签中。

```xml
<receiver android:name="co.cyberz.fox.FoxInstallReceiver" android:exported="true">
	<intent-filter>
		<action android:name="com.android.vending.INSTALL_REFERRER" />
	</intent-filter>
</receiver>
```

已经定义了"com.android.vending.INSTALL_REFERRER"的receiver类的话，请参考[让两种以上的INSTALL_REFERRER Receiver共存设置](./install_referrer/README.md)。

<div id="track_reengagement"></div>

## 流失唤回广告(Reengagement)计测的执行

为了计测经由自定义URL SCHEME的启动行为时，将所需设置添加在<application>标签中。
以下`IntentReceiverActivity`为F.O.X SDK中提供的Activity。

流失唤回广告通过自定义URL SCHEME呼出Activity来进行计测。
自定义URL SCHEME的设置请区别于其他Activity中已有内容。

```xml
<activity android:name="co.cyberz.fox.support.unity.IntentReceiverActivity">
	<intent-filter>
		<action android:name="android.intent.action.VIEW" />
		<category android:name="android.intent.category.DEFAULT" />
		<category android:name="android.intent.category.BROWSABLE" />
		<data android:scheme="自定义 URL SCHEME" />
	</intent-filter>
</activity>
```

<div id="receive_callback"></div>

## 取得Install计测完成的回调函数

[![F.O.X](http://img.shields.io/badge/F.O.X%20SDK-4.1.1%20〜-blue.svg?style=flat)](https://github.com/cyber-z/public-fox-unity-sdk/releases)

从4.1.1版及以后，希望取得Install计测完成的回调函数的话<br>
请一定按下方所示那样在UnityPlayerActivity(主Activity)的onResume里面执行`Fox.trackDeeplinkLaunch`方法。

```java
// Resume Unity
	@Override protected void onResume()
	{
		super.onResume();
		mUnityPlayer.resume();
		// FOX
		Fox.trackDeeplinkLaunch(this);
	}
```

> ※ 如果不按上述的方式来编码安装，在C#里将无法通知Install计测完成。

<div id="proguard"></div>

## 使用ProGuard

使用Proguard进行APP代码混淆时，为排除F.O.X SDK的方法，请添加以下设置。

```
-keepattributes *Annotation*
-keep class co.cyberz.** { *; }
-keep class com.google.android.gms.ads.identifier.* { *; }
-dontwarn co.cyberz.**
# 通过Gradle安装SDK时，不需要指定以下jar文件。
-libraryjars libs/FOX_Android_SDK_<VERSION>.jar

```

另外，在已安装Google Play Service SDK 的情况下，请确认以下页面中是否已记述keep指定。

> [Google Play Services导入时的Proguard应对(Android Developers)](https://developer.android.com/google/play-services/setup.html#Proguard)

<div id="others"></div>

## 其他

* [导入Google Play Services SDK来使用广告ID](./google_play_services/README.md)

* [AndroidManifest.xml设置案例](./config_android_manifest/AndroidManifest.xml)

* [（任意）利用外部储存优化重复排除](./external_storage/README.md)

* [（任意）Android M自动备份功能使用](./auto_backup/README.md)


---
[返回](../README.md)

[Top](../../../README.md)
