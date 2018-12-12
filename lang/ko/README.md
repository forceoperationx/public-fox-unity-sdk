# Force Operation X 란?

Force Operation X (이하 F.O.X)는 스마트폰의 광고 효과 최적화를 위한 토탈 솔루션 플랫폼 입니다. 애플리케이션의 다운로드, 웹상에서의 사용자 액션의 측정은 물론, 스마트폰 사용자의 행동 특성에 근거한 독자적인 효과측정기준을 바탕으로 기업의 프로모션의 비용효과를 극대화 할 수 있습니다.

본 문서에서는 스마트폰 애플리케이션의 광고 효과 극대화를 위한 F.O.X SDK 도입 단계에 대해 설명합니다.

## 목차

* **[1. 인스톨](#install_sdk)**
  * [SDK다운로드](https://github.com/cyber-z/public-fox-unity-sdk/releases)
  * [Unity 플러그인의 도입 방법](./doc/integration/README.md)
   * [iOS 프로젝트의 설정](./doc/integration/ios/README.md)
   * [Android 프로젝트의 설정](./doc/integration/android/README.md)
  * [최신 버전으로의 Migration에 대해서](./doc/update/README.md)
* **[2. F.O.X SDK의 액티베이션](#activate_sdk)**
* **[3. 인스톨 계측의 실장](#track_install)**
	*	[인스톨 계측의 상세](./doc/track_install/README.md)
* **[4. 애플리케이션내의 이벤트 계측](#track_event)**
	* [세션 (기동) 이벤트의 계측](#track_event)
	* [기타 애플리케이션내의 이벤트 계측](#track_other_event)
	* [이벤트 계측의 상세](./doc/track_event/README.md)
* **[5. 과거의 트러블 모음집(반드시 확인하여 주십시오)](#trouble_shooting)**

---

## F.O.X SDK란

F.O.X SDK를 애플리케이션에 도입함으로써 아래와 같은 기능을 제공합니다.

* **인스톨 계측**

광고 유입별 설치 횟수를 측정 할 수 있습니다.

* **LTV계측**

유입원 광고별로 Life Time Value를 측정합니다. 주요 성과 지점으로는 회원 가입, 튜토리얼 돌파, 과금등이 있습니다. 각 광고별 등록률, 과금률, 과금액 등을 측정 할 수 있습니다.

* **액세스 해석**

자연 유입과 광고 유입의 설치 비교, 애플리케이션의 기동수, 유니크 사용자수(DAU/MAU), 지속률 등을 측정 할 수 있습니다.


<div id="install_sdk"></div>

## 1. 인스톨

아래의 페이지에서 최신의 SDK를 다운로드 하십시오.

[SDK 릴리스 페이지] (https://github.com/cyber-z/public-fox-unity-sdk/releases)

이미 애플리케이션에 SDK가 설치되어있는 경우에는 [최신 버전으로의 Migration에 대해서](./doc/update/README.md)를 참조하십시오.

다운로드 한 SDK「FOX_UnityPlugin_<version>.zip」의 압축을 풀어 애플리케이션 프로젝트에 포함 시키십시오.

[Unity 플러그인의 설치 방법](./doc/integration/README.md)


### 각OS별 설정

* [iOS 프로젝트의 설정](./doc/integration/ios/README.md)
* [Android 프로젝트의 설정](./doc/integration/android/README.md)

<div id="activate_sdk"></div>

## 2. F.O.X SDK의 액티베이션

F.O.X SDK의 액티베이션을 실행하기 위하여 애플리케이션의 기동 시점에 이하의 실장을 행합니다.<br>
FoxConfig에 필수 사항을 대입하신후 `Fox.activate`를 실행합니다.

```cs
using Cyz;
...

void Start() {
	FoxConfig config = new FoxConfig ();
	config.iOSAppId = 발행된iOS용 APP_ID;
	config.iOSAppKey = 발행된 iOS용 APP_KEY;
	config.iOSAppSalt = 발행된 iOS용 APP_SALT;
	config.androidAppId = 발행된 Android용 APP_ID;
	config.androidAppKey = 발행된 Android용 APP_KEY;
	config.androidAppSalt = 발행된 Android용 APP_SALT;
	if(debug) config.isDebug = true;
	Fox.activate(config);
}
```

> ※ `isDebug`는 true로 설정하신경우 디버그용 로그를 출력합니다.


<div id="track_install"></div>

## 3. 인스톨 계측의 실장

첫회 기동의 인스톨 계측을 실장하여 광고의 효과 측정이 가능하게 됩니다.

### 인스톨 계측을 실장

인스톨 계측을 하기위해서는 기동시에 실행된는 스크립트에서 `Fox.trackInstall`을 호출합니다.

```cs
using Cyz;
...

	Fox.trackInstall();
```

*	[인스톨 계측의 상세](./doc/track_install/README.md)

<div id="track_event"></div>

## 4. 애플리케이션내 이벤트의 계측

기동 세션、회원 등록、튜토리얼 돌파、과금등 임의의 성과 지점에 이벤트 계측을 실장하여、유입별 광고의 LTV 및 잔존율을 측정 가능합니다. 계측을 원하지 않으실 경우에는 각 항목의 실장을 생략 가능합니다.

<div id="track_session"></div>

### 세션(기동)이벤트의 계측

자연 유입과 광고 유입의 인스톨 수 비교、애플리케이션의 기동수 및 유니크 유저수(DAU/MAU)、잔존율등을 계측하는 것이 가능합니다. 액세스 분석이 불필요한 경우 본 항목의 실장을 생략 가능합니다.
<br>
애플리케이션의 기동 또는 백 그라운드로 부터의 복귀할 때 세션 계측을 위한 코드를 추가합니다. 계측을 원하지 않으실 경우에는 각 항목의 실장을 생략 가능합니다.

```cs
using Cyz;
...

	Fox.trackSession();
```

<div id="track_other_event"></div>

### 기타 애플리케이션내 이벤트의 계측

회원 등록, 튜토리얼 돌파, 과금등 임의의 성과지점에 이벤트 계측을 실장하여 유입별 광고의 LTV를 계측 가능합니다.<br>
이벤트 계측이 불필요한 경우 본 항목의 실장을 생략 가능합니다.<br>
성과가 애플리케이션 내부에서 발생하는 경우 성과 처리부에 이하와 같이 기술하여 주십시오<br>

**[튜토리얼 이벤트의 계측 예]**
```cs
using Cyz;
...

	int ltvId = 성과 지점ID;
	FoxEvent e = new FoxEvent("_tutorial_comp", ltvId);
	e.buid = "USER_001"
	Fox.trackEvent(e);
```
LTV計測を行うためには、各成果地点を識別する成果地点IDを指定する必要があります。
イベント計測のみを計測する場合、イベント名だけ入力してください。


> LTV계측을 행하기 위해서는 각 성과지점을 식별하는 `성과 지점ID`를 지정하실 필요가 있습니다. 
> analytics이벤트 계측만을 원하실 경우 이벤트명만 입력하여 주십시오.

**[과금 이벤트의 계측 예]**

과금 계측을 하실 경우 과금이 완료된 지점에 이하와 같이 과금액을 지정하여 주십시오.

```cs
using Cyz;
...

	int ltvId = 성과 지점ID;
	double price = 1.2;
	String currency = "USD";
	FoxEvent purchase = FoxEvent.makePurchase("_purchase", ltvId, price, currency);
	purchase.buid = "USER_001"
	purchase.orderId = "ABCDEFG12345";
	purchase.itemName = "Coin";
	purchase.sku = "A-001"
	purchase.quantity = 1;
	Fox.trackEvent(purchase);
```

> currency의 지정시에는[ISO 4217](https://ko.wikipedia.org/wiki/ISO_4217)에 정의된 통화 코드를 지정하여 주십시오

* [이벤트 계측의 상세](./doc/track_event/README.md)

<div id="trouble_shooting"></div>

## 과거의 트러블 모음집(반드시 확인하여 주십시오)

### URL스키마 설정이 되지 않고 출시 되었기 때문에 브라우저에서 애플리케이션을 전환을 할 수 없습니다

Cookie 측정을 위해 외부 브라우저를 실행 한 후 원래 화면으로 되돌리려면 URL스키마를 이용하여 앱을 전환시킬 필요가 있습니다. 이 때 자신의 URL스키마가 설정되어 있어야 합니다. URL스키마를 설정하지 않고 출시하는 경우에는 이러한 전환을 할 수 없게됩니다.

### URL스키마에 대문자나 기호가 포함되어 성공적으로 앱에 전이되지 않는다

환경에 따라, URL스키마의 대소 문자를 판별 않음으로써 정상적으로 URL스키마의 전환 할수없는 경우가 있습니다. URL스키마는 모두 소문자와 숫자로 설정해야합니다.


### URL스키마 설정이 타사 앱과 동일하게 브라우저로부터 앱이 시작된다

iOS에서 복수의 앱에 동일한 URL 스키마가 설정되어있는 경우에는 어떤 앱이 시작하는지는 알 수 없습니다. 확실하게 특정 앱을 시작 할 수 없게 되므로 URL스키마는 타사 앱과는 유니크한 어느 정도의 복잡성이 있는것으로 설정하십시오.

### 단시간에 대량의 유저 획득을 하는 프로모션을 실시하면 제대로 측정이되지 않았다

iOS에는 앱 시작시 일정 시간 이상 메인 스레드가 차단되는 앱을 강제 종료하는 사양이 있습니다. 기동시 초기화 처리 등 메인 스레드에서 서버에 동기화 통신을 하지 않도록 주의하시기 바랍니다. 리워드 광고 등 대량의 사용자를 단시간에 획득한 결과, 서버에 대한 액세스가 집중시 통신 응답이 매우 나빠져 앱을 시작하는 데 시간이 걸려 처음 시작시 강제 종료되어 정상적으로 광고 성과가 측정 할 수 없게 된 사례가 있습니다.

아래 단계에서 이러한 상황을 테스트 할 수 있도록 아래의 설정에서 앱이 제대로 시작할지 여부를 확인하시기 바랍니다.

iOS「setting」 → 「developer」 → 「NETWORK LINK CONDITIONER」

* 「Enable」을 ON
* 「Very Bad Network」를 체크


### F.O.X에서 확인할 수 있는 설치수의 값이 Google Play Developer Console의 숫자보다 많을때

F.O.X는 몇개의 방식을 조합하여 단말기의 중복 설치 검사를 실시하고 있습니다.
중복 감지 할 수 없는 설정은 동일한 단말기에서도 다시 설치 될 때마다 F.O.X는 신규 설치라고 판정해 버립니다.

중복 탐지의 정확성을 향상시키기 위해 아래와 같이 설정하십시오.

* [광고 ID를 이용하기 위한 Google Play Services SDK의 도입](./doc/google_play_services/README.md)

* [(옵션)외부 스토리지를 이용한 중복 제거 설정](./doc/external_storage/README.md)

* [（옵션）Android M 오토 백업 기능의 이용](./doc/integration/android/auto_backup/README.md)


---
[톱 메뉴](/README.md)
