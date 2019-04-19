[TOP](../../../README.md)　>　[Unity 플러그인의 도입 절차](../README.md)　>　**Android 프로젝트의 상세 설정**

---

# Android 프로젝트의 상세 설정

* [Gradle 경유의 도입](#install_by_gradle)
* [AndroidManifest.xml의 샘플에 대하여](#sample_manifest)
* [퍼밋션의 설정](#permission)
* [인스톨 리퍼러 계측의 설정](#install_referrer)
* [리엔게이지멘트 계측의 실장](#track_reengagement)
* [ProGuard를 이용하는 경우](#proguard)
* [기타](#others)

<div id="install_by_gradle"></div>

## Gradle 경유의 도입

Android Studio 프로젝트에서 Gradle를 이용하여 SDK를 도압하는 경우 이하의 Dependencies를 설정합니다.

```groovy
repositories {
    maven {
        url "https://github.com/forceoperationx/public-fox-android-sdk/raw/master/mavenRepo"
    }
}

dependencies {
    compile 'co.cyberz.fox:track-core:4.0.0'
    compile 'co.cyberz.fox.support:track-unity:1.0.0'
}
```


<div id="sample_manifest"></div>

## AndroidManifest.xml의 샘플에 대하여

Android용의 설정은 Unity프로젝트상에서 하실수 있습니다. Unity프로젝트에 도입된
AndroidManifest.xml를 편집합니다.<br>프로젝트에 AndroidManifest.xml이 존재하지 않는 경우에는「Plugins/Android/AndroidManifest-sample.xml」를 「AndroidManifest.xml」로 다른이름 저장하여 사용하여 주십시오

<div id="permission"></div>

## 퍼밋션의 설정

F.O.X SDK에서는 아래의 3가지 퍼밋션을 이용합니다.
&lt;Manifest&gt;태그내에 다음의 퍼밋션 설정을 추가합니다.

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

퍼밋션|Protection Level|필수|개요
:---|:---:|:---:|:---
INTERNET|Normal|필수|F.O.X SDK가 통신을 하기 위하여 필요합니다.
READ_EXTERNAL_STORAGE|Dangerous|임의|스토레지를 이용한 중복 배제 기능 향상에 필요합니다.(※1)
WRITE_EXTERNAL_STORAGE|Dangerous|임의|스토레지를 이용한 중복 배제 기능 향상에 필요합니다.(※1)

> ※1 Android M부터 ProtectionLevel이 `dangerous`로 지정되어 있는 퍼밋션을 필요로하는 기능을 이용하기 위해서는 유저의 허가가 필요합니다. 상세는 [외부 스토레지를 이용한 중복 배제 설정](./external_storage/README.md)을 확인하여 주십시오.

<div id="install_referrer"></div>

## 인스톨 리퍼러 계측의 설정
인스톨 리퍼러를 이용한 인스톨 계측을 하기 위하여 아래의 설정을 &lt;application&gt;태그에 추가합니다.

```xml
<receiver android:name="co.cyberz.fox.FoxInstallReceiver" android:exported="true">
	<intent-filter>
		<action android:name="com.android.vending.INSTALL_REFERRER" />
	</intent-filter>
</receiver>
```

이미 "com.android.vending.INSTALL_REFERRER"에 대하여 레시버 클래스를 정의 하신 경우에는 [둘 이상의 INSTALL_REFERRER 레시버를 공존시키기 위한 설정](./install_referrer/README.md)을 참고하여 주십시오.

<div id="track_reengagement"></div>

## 리엔게이지멘트 계측의 실장

커스텀 URL스킴 경유 기동을 계측하기 위해 필요한 설정을<application>태그내에 추가합니다.
이하의 `IntentReceiverActivity`는 F.O.X SDK에서 제공하고 있는 Activity입니다.

커스텀 URL스킴으로 이 Activity를 호출하는 것으로 리엔게이지멘트 계측을 합니다.
이곳의 커스텀 URL스킴은 다른 Activity에 설정하고 있는 스킴과는 다른 값을 설정하여 주십시오.

```xml
<activity android:name="co.cyberz.fox.support.unity.IntentReceiverActivity">
	<intent-filter>
		<action android:name="android.intent.action.VIEW" />
		<category android:name="android.intent.category.DEFAULT" />
		<category android:name="android.intent.category.BROWSABLE" />
		<data android:scheme="커스텀 URL 스킴" />
	</intent-filter>
</activity>
```

<div id="receive_callback"></div>

## 인스톨 계측완료의 콜백을 받습니다.

[![F.O.X](http://img.shields.io/badge/F.O.X%20SDK-4.1.1%20〜-blue.svg?style=flat)](https://github.com/forceoperationx/public-fox-unity-sdk/releases)

버전 4.1.1이후에 인스톨 계측 완료의 콜백을 받을 경우에는<br>
이하와 같이 반드시 UnityPlayerActivity(메인의 액티비티)의 onResume에 `Fox.trackDeeplinkLaunch`메소드를 실장하여 주십시오.

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

> ※ 본 실장이 되어있지 않는 경우 C#에 인스톨 계측 완료가 통지 되지 않습니다.

<div id="proguard"></div>

## ProGuard를 이용하는 경우

ProGuard를 이용하여 애플리케이션의 난독화를 하는 경우 F.O.X SDK의 메소드가 대상이 되지 않도록 이하의 설정을 추가하여 주십시오.

```
-keepattributes *Annotation*
-keep class co.cyberz.** { *; }
-keep class com.google.android.gms.ads.identifier.* { *; }
-dontwarn co.cyberz.**
# Gradle경유로 SDK를 인스톨하는 경우 아래의 jar파일의 지정은 불필요합니다.
-libraryjars libs/FOX_Android_SDK_<VERSION>.jar

```

또는 Google Play Service SDK를 도입하고 있는 경우에는 이하의 페이지에 기재되어 있는 keep지정이 기술되어 있는지를 확인하여 주십시오

> [Google Play Services 도입시의 Proguard 대응 (Android Developers)](https://developer.android.com/google/play-services/setup.html#Proguard)

<div id="others"></div>

## 기타

* [광고 식별자 ID를 이용하기 위한 Google Play Services SDK의 도입](./google_play_services/README.md)

* [AndroidManifest.xml 설정 샘플](./config_android_manifest/AndroidManifest.xml)

* [（옵션）외부 스토레지를 이용한 중복 배제 설정](./external_storage/README.md)

* [（옵션）Android M 오토 백업 기능의 이용](./auto_backup/README.md)


---
[돌아가기](../README.md)

[톱](../../../README.md)
