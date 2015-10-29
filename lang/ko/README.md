# Force Opetaion X 란?

Force Operation X (이하 F.O.X)는 스마트폰의 광고 효과 최적화를 위한 토탈 솔루션 플랫폼 입니다. 앱의 다운로드, 웹상에서의 사용자 액션의 측정은 물론, 스마트폰 사용자의 행동 특성에 근거한 독자적인 효과측정기준을 바탕으로 기업의 프로모션의 비용효과를 극대화 할 수 있습니다.

본 문서에서는 스마트폰 앱의 광고 효과 극대화를 위한 F.O.X SDK 도입 단계에 대해 설명합니다.

## F.O.X SDK 란?

F.O.X SDK를 앱에 도입함으로써 아래와 같은 기능을 제공합니다.

* **설치 계측**

광고 유입별 설치 횟수를 측정 할 수 있습니다.

* **LTV 측정**

유입원 광고별로 Life Time Value를 측정합니다. 주요 성과 지점으로는 회원 가입, 튜토리얼 돌파, 과금등이 있습니다. 각 광고별 등록률, 과금률, 과금액 등을 측정 할 수 있습니다.

* **액세스 해석**

자연 유입과 광고 유입의 설치 비교, 앱의 기동수, 유니크 사용자수(DAU/MAU), 지속률 등을 측정 할 수 있습니다.

* **푸시 알림**

F.O.X에서 측정된 정보를 사용하여 사용자에게 푸시 알림을 할 수 있습니다. 예를 들어 특정 광고에서 유입된 사용자에게 메시지를 보낼 수 있습니다.

## 1. 설치

아래의 페이지에서 최신의 SDK를 다운로드 하십시오.

[SDK 릴리스 페이지] (https://github.com/cyber-z/public-fox-unity-sdk/releases)

이미 앱에 SDK가 설치되어있는 경우에는 [최신 버전에 대한 업데이트에 대해](./doc/update/README.md)를 참조하십시오.

다운로드 한 SDK「FOX_UnityPlugin_<version>.zip」의 압축을 풀어 앱 프로젝트에 포함 시키십시오.

[Unity 플러그인의 설치 방법](./doc/integration/README.md)


## 2. 설치 계측 구현

처음 시작할 설치 계측을 구현하여 광고의 효과 측정을 할 수 있습니다. 아래의 하나를 수행하여 구현해야합니다.

* GUI(Inspector)에서 설치 측정을 설정
* 코드를 작성


### GUI (Inspector)에서 설치 계측 구현

앱 기동시 한번만 읽어들이는 오브젝트가 있는 경우는 GUI(Inspector)에서 편집이 가능합니다.

예) Main Camera의 Inspector를 이용하여 설치 측정을 수행

1. 프로젝트의 「Plugins/FoxPlugin.cs」를 Main Camera에 드래그 앤 드롭
2. Main Camera의 Inspector에서 Fox Plugin 스크립트의 Url변수에 default라는 문자열을 지정


### 코드를 작성하는 경우의 설치 계측 구현

GUI(Inspector)를 이용하지 않고 스크립트로 설치 측정 프로세스에 대한 설명을 할 경우에는 기동시 실행되는 스크립트로부터 FoxPlugin.sendConversion를 호출합니다.

```cs
FoxPlugin.sendConversion("default");
```

sendConversion의 인수에는 일반적으로 위와 같이 "default"라는 문자열을 입력하십시오.

특정 URL에 이동 시키고 싶은 경우나 앱에서 동적으로 URL을 생성하고자하는 경우에는 URL 문자열을 설정하십시오.

```cs
FoxPlugin.sendConversion("http://yourhost.com/yourpage.html");
```

## 3. LTV 측정의 구현

회원등록, 튜토리얼 돌파, 결제등 임의의 성과 지점에 LTV 측정을 구현함으로써 유입원 광고의 LTV를 측정 할 수 있습니다. LTV측정이 필요없는 경우에는 본 항목의 구현을 생략 할 수 있습니다.

소스의 편집은 성과가 오른 후에 실행되는 스크립트에 처리를 기술합니다. 예를 들어 회원 가입이나 앱 내 과금후의 과금측정에서는 등록·과금 처리 실행 후 콜백에 LTV 측정 처리를 기술합니다. 대상 스크립트(C# 또는 JavaScript)에 의해 편집 내용이 다르기 때문에 주의 하십시오.


```cs
FoxPlugin.sendLtv(성과지점 ID);
```

LTV 측정을하기 위해서는 각 성과 지점을 식별하는 성과 지점 ID를 지정해야합니다. sendLtv 인수에 발행된 ID를 지정하십시오.

과금 측정을 할 경우에는 결제가 완료된 부분에 아래와 같이 결제 금액을 지정하십시오.

```cs
// ...
FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");
FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "20");
FoxPlugin.sendLtv(성과지점 ID);
```

> Javascript에서 편집 할 경우, 문장 안의 「FoxPlugin」을 「FoxPluginJS」로 바꿉니다.

* [sendLtv 상세 정보] (./doc/send_ltv_conversion/README.md)

## 4. 액세스 해석의 구현

자연 유입과 광고 유입의 설치수 비교, 앱의 기동수와 유니크 방문자수(DAU/MAU), 지속률 등을 측정 할 수 있습니다. 액세스 해석이 불필요한 경우에는 본 항목의 구현을 생략 할 수 있습니다.

액세스 해석에 의한 측정을 할 경우, 아래 중 하나를 수행하여 구현해야합니다.

* 스크립트를 이용하기
* 코드를 작성하기

### 스크립트를 이용하는 경우

「Plugins/FoxAnalyticsSession」를 Main Camera에 드래그 앤 드롭 합니다.
앱 기동시 또는 백그라운드에서 복귀시에 세션개시측정을 합니다.

> 프로젝트내에 복수의Scene이 존재하는 경우는 측정 지점은 모든 Main Camera에 설정하십시오. 설정되지 않은 Scene이 표시되는 상태에서 백그라운드 또는 복귀한 경우에는 정확한 측정을 할 수 없게 됩니다.


### 코드를 작성하는 경우

앱의 기동 지점에서 다음 메소드를 구현합니다.

```cs
FoxPlugin.sendStartSession();
```



* **액세스 해석에 의한 과금 측정**
액세스 해석에 의한 과금 측정을 수행하려면 아래 링크를 참조하십시오.

[액세스 해석에 의한 이벤트 측정] (./doc/analytics_event/README.md)


## 5. 각 OS마다 설정

* **iOS 용 Xcode 프로젝트 설정**

### Xcode 프로젝트의 퍼블리시

iOS 용 프로젝트를 생성하기 위해 다음과 같이 Xcode 프로젝트를 게시하고 Xcode에서 필요한 설정을 합니다.

1. 메뉴의 「File」 > 「BUild Settings ...」를 선택
2. Platform의 「iOS」를 선택하고 「Switch Platform」을 클릭
3. 「Player Settings」를 클릭해 Inspector에서 자신의 환경에 맞게 설정을 함
4. 「Build」또는 「Build And Run」을 클릭하여 Xcode 프로젝트의 퍼블리시를 함


### Xcode 프로젝트의 편집

퍼블리시 된 Xcode프로젝트를 열고 편집합니다.

* **프레임 워크 설정**

다음 프레임 워크를 프로젝트에 연결하십시오.

<table>
<tr><th>프레임 워크명</th><th>Status</th></tr>
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>iAd.framework </td><td>Required</td></tr>
<tr><td>Security.framework </td><td>Required </td></tr>
<tr><td>StoreKit.framework </td><td>Required </td></tr>
<tr><td>SystemConfiguration.framework</td><td>Required</td></tr>
</table>

> AdSupport.framework은 iOS 6 이후에 추가된 프레임 워크를 위해, 앱을 iOS 5 이하에서도 기동 시키는(iOS Deployment Target을 5.1 이하로 설정하는) 경우에는 weak link를 실행하기 위해 "Optional"로 설정 하십시오.

![프레임 워크 설정 01] (./doc/config_framework/img01.png)

[프레임 워크 설정의 상세](./doc/config_framework/README.md)

* **SDK 설정**

SDK의 동작에 필요한 설정을 plist에 추가합니다. 「AppAdForce.plist」파일을 프로젝트의 임의에 위치에 생성하고 다음의 키와 값을 입력하십시오.

<table>
<tr>
  <th>Key</th>
  <th>Type</th>
  <th>Value</th>
</tr>
<tr>
  <td>APP_ID</td>
  <td>String</td>
  <td>Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오.</td>
</tr>
<tr>
  <td>SERVER_URL</td>
  <td>String</td>
  <td>Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오.</td>
</tr>
<tr>
  <td>APP_SALT</td>
  <td>String</td>
  <td>Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오.</td>
</tr>
<tr>
  <td>APP_OPTIONS</td>
  <td>String</td>
  <td>아무것도 입력하지 마시고 빈문자의 상태로 해 주십시오.</td>
</tr>
<tr>
  <td>CONVERSION_MODE</td>
  <td>String</td>
  <td>1</td>
</tr>
<tr>
  <td>ANALYTICS_APP_KEY</td>
  <td>String</td>
  <td>Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오.<br />액세스 해석을 이용하지 않으면 설정은 필요 없습니다.</td>
</tr>
</table>

![프레임 워크 설정 01](./doc/config_plist/img05.png)

[SDK설정의 상세](./doc/config_plist/README.md)

[AppAdForce.plist 샘플](./doc/config_plist/AppAdForce.plist)


* **Android용 프로젝트의 설정**

Android 용 설정은 Unity 프로젝트에서 할 수 있습니다. Unity 프로젝트에 포함된
AndroidManifest.xml를 편집합니다. 프로젝트에 AndroidManifest.xml이 존재하지 않는 경우는 「Plugins/Android/AndroidManifest-sample.xml」를 「AndroidManifest.xml」로 수정하여 사용하십시오.


### 권한 설정

<Manifest> 태그 내에 다음의 권한 설정을 추가합니다.

```xml:
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAG​​E"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAG​​E"/>
```

### 메타 데이터의 설정

SDK의 실행에 필요한 정보를<application>태그내에 추가합니다.

```xml:
<meta-data android:name ="APPADFORCE_APP_ID" android:value="Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오."/>
<meta-data android:name = "APPADFORCE_SERVER_URL"　android:value="Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오."/>
<meta-data android:name = "APPADFORCE_CRYPTO_SALT" android:value="Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오."/>
<meta-data android:name = "ANALYTICS_APP_KEY" android:value="Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오."/>
```

설정할 key와value는 다음과 같습니다.

|파라메터명|필수|개요|
|:------|:------|:------|
|APPADFORCE_APP_ID|필수|Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오.|
|APPADFORCE_SERVER_URL|필수|Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오.|
|APPADFORCE_CRYPTO_SALT|필수|Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오.|
|ANALYTICS_APP_KEY|필수|Force Operation X 관리자가 연락합니다. 그 값을 입력하십시오.|


### 설치 referer측정 설정
설치 referer를 이용한 설치 측정을 위해 아래와 같은 설정을 <application> 태그에 추가합니다.

```xml:
<receiver android:name="jp.appAdForce.android.InstallReceiver" android:exported="true">
  <intent-filter>
    <action android : name = "com.android.vending.INSTALL_REFERRER"/>
  </intent-filter>
</receiver>
```

이미 "com.android.vending.INSTALL_REFERRER"에 대한 receiver 클래스가 정의되어있는 경우에는, [두개의 INSTALL_REFERRER receiver를 공존시키는 경우 설정](./doc/install_referrer/README.md)를 참조하십시오.


### Engagement 측정의 구현

사용자 정의 URL 스키마 경유의 기동을 측정하기 위해 필요한 설정을<application>태그 내에 추가 합니다.
사용자 정의 URL 체계는 다른 Activity에서 설정한 것과 다른 값을 설정하십시오.

```xml:
<activity android:name="jp.appAdForce.android.IntentReceiverActivity">
  <intent-filter>
    <action android : name = "android.intent.action.VIEW"/>
      <category android : name = "android.intent.category.DEFAULT"/>
        <category android : name = "android.intent.category.BROWSABLE"/>
        <data android : scheme = "사용자 정의 URL 스키마"/>
  </intent-filter>
</activity>
```

[광고 ID를 이용하기 위한 Google Play Services SDK의 도입](./doc/google_play_services/README.md)

[(옵션) 외부 스토리지를 이용한 중복 제거 설정](./doc/external_storage/README.md)

[AndroidManifest.xml 샘플](./doc/config_android_manifest/AndroidManifest.xml)


## 6. ProGuard를 사용하는 경우

ProGuard를 이용하여 앱의 난독화 처리를 할 때는 F.O.X SDK의 메소드가 대상이되지 않도록 아래의 설정을 추가 하십시오.

```
-keepattributes * Annotation *

-libraryjars libs / AppAdForce.jar
-keep interface jp.appAdForce ** {*;}
-keep class jp.appAdForce ** {*;}
-keep class jp.co.dimage ** {*;}
-keep class com.google.android.gms.ads.identifier * {*;}
-dontwarn jp.appAdForce.android.ane.AppAdForceContext
-dontwarn jp.appAdForce.android.ane.AppAdForceExtension
-dontwarn com.adobe.fre.FREContext
-dontwarn com.adobe.fre.FREExtension
-dontwarn com.adobe.fre.FREFunction
-dontwarn com.adobe.fre.FREObject
-dontwarn com.ansca **
-dontwarn com.naef.jnlua **
```

또한 Google Play Service SDK를 도입되는 경우는 다음 페이지에 기재되어 keep 지정이 기술되어 있는지 확인하십시오.

[Google Play Services 도입시 Proguard 지원](https://developer.android.com/google/play-services/setup.html#Proguard)


## 소통 테스트 실시

마켓에 신청까지 SDK를 도입 한 상태에서 충분히 테스트하여 앱의 동작에 문제가 없는지 확인하십시오.

설치 계측 통신은 시작한 후 한 번만 실행합니다. 계속해서 효과 측정 테스트를 실시하고 싶은 경우에는 앱을 제거하고 다시 설치 후 사용하십시오.

* **테스트 단계**

1. 테스트 용 단말기에 테스트 앱이 설치되어있는 경우에는 제거
1. 테스트 용 단말기의 기본 브라우저의 Cookie를 삭제
1. 당사에서 발행한 테스트 용 URL을 클릭
1. 마켓에 리다이렉션
1. 테스트 용 단말기에 테스트 앱 을 설치 <br />
1. 앱을 기동, 브라우저가 기동<br />
여기에서 브라우저가 시작되지 않는 경우에는 정상적으로 설정이 이루어지고 있지 않습니다. 설정을 검토하여 주셔서 문제가 없을 경우에는 당사에 문의하십시오.
1. LTV 지점까지 화면 전환<br />
1. 앱을 종료하고 백그라운드에서도 삭제<br />
1. 다시 앱을 시작<br />

당사에 3,6,7,9 시간을 알려 주십시오. 제대로 측정이 이루어지고 있는지 확인합니다. 당사 측의 확인에서 문제가 없다면 테스트 완료됩니다.

> 테스트 용 URL은 반드시 단말기의 기본 브라우저에서 요청되도록 하십시오. 메일 앱과 QR 코드 앱을 사용해 앱에서 WebView로 전환한 경우에는 측정 할 수 없습니다.

> 테스트 URL을 클릭했을 때 전환 대상이 아닌 오류 메시지가 표시되는 경우가 있습니다만 소통 테스트에서는 문제 없습니다.


[Engagement측정을 할 경우 테스트 단계](./doc/reengagement_test/README.md)


## 기타 기능의 구현

* [푸시 알림 구현](./doc/notify/README.md)

* [수신 거부의 구현](./doc/optout/README.md)

* [관리 화면에 등록한 번들 버전에 따른 처리의 배분](./doc/check_version/README.md)


## 마지막에 반드시 확인하시기 바랍니다 (지금까지 발생한 문제점들)

### URL스키마 설정이 되지 않고 출시 되었기 때문에 브라우저에서 앱을 전환을 할수없습니다

Cookie 측정을 위해 외부 브라우저를 실행 한 후 원래 화면으로 되돌리려면 URL스키마를 이용하여 앱을 전환시킬 필요가 있습니다. 이 때 자신의 URL스키마가 설정되어 있어야 합니다. URL스키마를 설정하지 않고 출시하는 경우에는 이러한 전환을 할 수 없게됩니다.

### URL스키마에 대문자나 기호가 포함되어 성공적으로 앱에 전이되지 않는다

환경에 따라, URL스키마의 대소 문자를 판별 않음으로써 정상적으로 URL스키마의 전환 할수없는 경우가 있습니다. URL스키마는 모두 소문자와 숫자로 설정해야합니다.


### URL스키마 설정이 타사 앱과 동일하게 브라우저로부터 앱이 시작된다

iOS에서 복수의 앱에 동일한 URL 스키마가 설정되어있는 경우에는 어떤 앱이 시작하는지는 알 수 없습니다. 확실하게 특정 앱을 시작 할 수 없게 되므로 URL스키마는 타사 앱과는 유니크한 어느 정도의 복잡성이 있는것으로 설정하십시오.

### 단시간에 대량의 유저 획득을 하는 프로모션을 실시하면 제대로 측정이되지 않았다

iOS에는 앱 시작시 일정 시간 이상 메인 스레드가 차단되는 앱을 강제 종료하는 사양이 있습니다.기동시 초기화 처리 등 메인 스레드에서 서버에 동기화 통신을 하지 않도록 주의하시기 바랍니다. 리워드 광고 등 대량의 사용자를 단시간에 획득한 결과, 서버에 대한 액세스가 집중시 통신 응답이 매우 나빠져  앱을 시작하는 데 시간이 걸려 처음 시작시 강제 종료되어 정상적으로 광고 성과가 측정 할 수 없게 된 사례가 있습니다.

아래 단계에서 이러한 상황을 테스트 할 수 있도록 아래의 설정에서 앱이 제대로 시작할지 여부를 확인하시기 바랍니다.

iOS「설정」 → 「개발자」 → 「NETWORK LINK CONDITIONER」

* 「Enable」을 ON
* 「Very Bad Network」를 체크


### F.O.X에서 확인할 수 있는 설치수의 값이 Google Play Developer Console의 숫자보다 많을때

F.O.X는 몇개의 방식을 조합하여 단말기의 중복 설치 검사를 실시하고 있습니다.
중복 감지 할 수 없는 설정은 동일한 단말기에서도 다시 설치 될 때마다 F.O.X는 신규 설치라고 판정해 버립니다.

중복 탐지의 정확성을 향상시키기 위해 아래와 같이 설정하십시오.

* [광고 ID를 이용하기 위한 Google Play Services SDK의 도입](./doc/google_play_services/README.md)

* [(옵션)외부 스토리지를 이용한 중복 제거 설정](./doc/external_storage/README.md)
