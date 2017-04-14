[TOP](../../README.md)　>　**이벤트 계측의 상세**

---

# 이벤트 계측의 상세

trackEvent메소드를 이용하여 광고 유입별 과금 금액 및 회원가입수 등을 계측 가능합니다. 계측을 위하여 임의의 지점에 LTV성과 통신을 위한 코드를 추가합니다.

소스의 편집은 성과를 올린후에 실행되는 스크립트에 처리를 기술합니다. 예를들어 회원등록 및 애플리케이션내 과금후의 과금계측에서는 등록 과금 처리 실행후의 콜백내에 이벤트 계측 처리를 기술합니다.

```cs
using Cyz;
...

FoxEvent e = new FoxEvent(이벤트명, 성과 지점ID);
Fox.trackEvent(e);
```

> 이벤트명(필수) : 임의의 이벤트명을 지정할 수 있습니다.

> 성과 지점ID(필수) : 관리콘솔의 성과지점설정에서 확인 가능합니다.

<div id="add_buid"></div>

### Buid (광고주 단말 ID)를 지정합니다.

애플리케이션 내부의 성과에 광고주 단말 ID（회원 ID등）를 포함하는 것이 가능하여 이것을 기준으로 성과 계측이 행해 집니다. LTV성과에 광고주 단말ID를 부여하는 경우 이하와 같이 기술하여 주십시오.

```cs
using Cyz;
...

	FoxEvent e = new FoxEvent(이벤트명, 성과 지점ID);
	e.buid = "광고주 단말ID";
	Fox.trackEvent(e);
```

> 광고주 단말ID(옵션)：광고주가 관리하고 있는 유니크한 식별자（회원 ID등）입니다.
지정 가능한 값은 64문자 이내의 영숫자 입니다.

<div id="add_params"></div>

### 임의의 파라메터를 지정합니다.

애플리케이션내 계측시에는 파라메터를 옵션으로서 지정 가능합니다.

```cs
using Cyz;
...

	FoxEvent e = new FoxEvent(이벤트명, 성과 지점ID);
	e.addParam("파라메터명", "값");
	Fox.trackEvent(e);
```

<div id="purchase"></div>

### 과금 이벤트 실장 예

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

FoxEvent에 설정 가능한 파라메터의 사양은 아래와 같습니다.

|파라메터|type|최대 길이|개요|
|:------|:------:|:------:|:------|
|eventName|String|255|트랙킹을 행하는 이벤트를 식별하기 위한 명칭을 설정합니다. 이벤트명은 자유로이 설정 가능합니다.|
|orderId|String|255|주문번호를 지정합니다.|
|sku|String|255|상품 코드등을 지정합니다.|
|itemName|String|255|상품명을 지정합니다.|
|price|double||상품단가를 지정합니다.|
|quantity|int||수량을 지정합니다. price * quantity가 매출 금액으로써 계상됩니다.|
|currency|String||통화 코드를 지정합니다. 미지정의 경우는 "JPY"가 지정됩니다.|

> currency에는 [ISO 4217](http://ko.wikipedia.org/wiki/ISO_4217)에 정의된 통화 코드를 지정하여 주십시오.

<div id="session"></div>

### 세션 이벤트

애플리케이션이 기동 또는 백그라운드에서 복귀하는 경우 세션 계측을 하는 코드를 추가합니다. 불필요한 경우에는 본 항목의 실장을 생략 가능합니다.

```cs
using Cyz;
...

	Fox.trackSession();
```

#### (Android) Java에 의한 실장

Android의 경우 이하와 같이 java에 의한 실장도 가능합니다.<br>
애플리케이션의 기동 및 백 그라운드에서의 복귀를 계측하기 위하여 Activity의 `onResume`메소드에 코드를 추가합니다.

애플리케이션 기동시의 기동 계측（MainActivity클래스에의 실장 예）
```java
import co.cyberz.fox.Fox;

public class MainActivity extends Activity {

		@Override
		protected void onResume() {
            super.onResume();
            Fox.trackSession();
		}
}
```
> ※ 애플리케이션이 백그라운드에서 복위하는 경우 그 Activity에 기동 계측이 실장되어 있지 않는 경우에는 정확한 액티브 유저수가 계측되지 않게 됩니다.

> ※ Java와 C#의 양측에서 trackSession()가 실장되어 있는 경우 １유저에서 2중의 기동 정보가 송신되므로 반드시 어느 한쪽에만 실장하여 주십시오.

#### 기타

* [엔게이지멘트 계측](./engagement/README.md)


---
[톱](../../README.md)
