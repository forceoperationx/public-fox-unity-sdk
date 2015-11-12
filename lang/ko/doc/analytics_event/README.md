## 액세스 해석에 의한 청구 측정

액세스 해석 기능을 이용하여 자연 유입 경유를 포함한 광고별 이벤트와 매출을 각각 측정 할 수 있습니다. 액세스 해석에 의한 측정을 위해 다음 sendEvent 메소드를 구현합니다.

튜토리얼 돌파와 회원 등록 등의 이벤트 계측의 경우에는 다음과 같이 설명합니다.

```cs
FoxPlugin.sendEvent(eventName, action, label, quantity);
```

과금 측정의 경우에는 아래와 같이 설명합니다.

```cs
FoxPlugin.sendEventPurchase(eventName, action, label, orderId, sku, itemName, price, quantity, currency);
```

sendEvent 메소드의 파라미터의 사양은 아래와 같습니다.

|매개 변수|형|최대 길이|개요|
|:------|:------:|:------:|:------|
|eventName|String|255|트래킹을 하는 이벤트를 식별 할 수있는 임의의 이름을 설정합니다. 이벤트명은 자유롭게 설정 가능합니다.|
|action|String|255|이벤트에 속하는 액션명을 설정합니다. 액션명은 자유롭게 설정 가능합니다. 특히 지정이없는 경우에는 null도 관계 없습니다.|
|label|String|255|액션에 속한 라벨명을 설정합니다. 라벨명은 자유롭게 설정 가능합니다. 특히 지정이없는 경우에는 null도 관계 없습니다.|
|orderId|String|255|주문 번호 등을 지정합니다. 특히 지정이없는 경우에는 null도 관계 없습니다.|
|sku|String|255|상품 코드 등을 지정합니다. 특히 지정이없는 경우는 null에서도 상관하지 않습니다.|
|itemName|String|255|상품명을 지정합니다. 지정이없는 경우는 비어 두십시오.("")
|price|double||상품 단가를 지정합니다.|
|quantity|int||수량을 지정합니다. price * quantity가 매출액으로 계상됩니다.|
|currency|String||통화 코드를 지정합니다. null의 경우 "JPY"가 지정됩니다.|

> currency에는[ISO 4217] (http://ja.wikipedia.org/wiki/ISO_4217)에서 정의된 통화코드를 지정 하십시오.

LTV 측정에서도 과금을 성과 지점으로하는 경우에는 동일한 부분에 LTV와 액세스 해석의 각각의 측정 처리를 구현합니다.

샘플은 아래의 미국달러로 3달러 결제를 한 경우의 구현 예를 기재합니다.



```cs
// LTV계측에 의한 과금 측정
FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");
FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "3");
FoxPlugin.sendLtv(성과지점 ID);

// 액세스 해석에 의한 과금 측정
FoxPlugin.sendEventPurchase("purchase", null, null, null, null, "", 3, 1, "USD");
```

---
[TOP](/lang/ko/doc/README.md)
