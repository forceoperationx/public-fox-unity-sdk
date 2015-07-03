## sendLtv 상세

sendLtvConversion 메소드를 이용하여 광고 유입별 과금 금액 및 가입 횟수 등을 측정 할 수 있습니다. 측정을 위해 임의의 지점에 LTV성과통신을 하는 코드를 추가합니다.

소스의 편집은, 성과가 오른 후에 실행된 스크립트에 처리를 기술합니다. 예를 들어, 회원 등록이나 앱 내 과금 후의 과금 측정에서는 등록·과금 처리 실행 후 콜백 내에 LTV 측정 처리를 설명합니다.
대상 스크립트(C# 또는 JavaScript)에 의해 편집 내용이 다르므로 주의하시기 바랍니다.

성과가 앱 내부에서 발생하는 경우 성과 처리부에 아래의 C#으로 작성합니다.
JavaScript에 편집할 경우는 문장내의 「FoxPlugin」을 「FoxPluginJS」로 바꾸어 주십시오

성과 통지 코드를 추가

```C #
FoxPlugin.sendLtv(성과 지점 ID);
```
> 성과 지점ID(필수) : 관리자가 연락합니다. 그 값을 입력하십시오.


앱 내부의 성과에 광고주 단말 ID(회원ID 등)를 포함 할 수 있으며 이를 기준으로 한 성과 측정이 일어납니다. LTV 성과에 광고주 단말 ID를 부여하려면 다음과 같이 설명합니다.

```C#
	FoxPlugin.sendLtv (성과지점ID, "광고주 단말ID");
```

> 성과 지점 ID(필수) : 관리자에의해 연락합니다. 그 값을 입력하십시오.
광고주 단말 ID(옵션) : 광고주가 관리하는 고유 식별자(회원 ID) 입니다.
지정할수 있는값은 64자 이내의 영숫자입니다.


앱 내 측정시에는 매개변수를 옵션으로 설정하는 것이 가능합니다.

```C#
FoxPlugin.addParameter("매개변수 명", "값");
```

지정할 수있는 매개 변수는 다음과 같습니다.

|매개 변수명|소개|
|:------|:------|
|PARAM_SKU|Stock Keeping Unit (상품 관리 코드)<br> 
(영숫자 32 문자까지)<br>상품의 재고 관리를 할 때 사용하십시오|
|PARAM_PRICE|Price<br>(정수 한화) <br>
매출액을 관리하는데 사용하십시오. |
|PARAM_CURRENCY|Currency<br>(반각 영문자 3자 통화 코드) <br> 
통화 별 과금 액을 집계 할 때 사용합니다. <br> 
통화가 설정되어 있지 않은 경우, Price를 JPY (일본 엔)로 처리합니다.|
|임의로 매개 변수를 추가하는 것도 가능합니다.|FoxPlugin.addParameter(
"파라메타명", "값");
<br>※1
동일한 파라메터명을 작성하는 경우는 후자가 활성화됩니다. <br>※2
밑줄 ("_")를 매개 변수 이름 앞에 설명하지 마십시오. <br> ※3
영숫자 이외는 사용할 수 없습니다.|

PARAM_CURRENCY에는[ISO 4217](http://ja.wikipedia.org/wiki/ISO_4217)에서 정의된 통화 코드를 지정하십시오.

설정 예 :
```C#
FoxPlugin.addParameter(PARAM_SKU, "ABC1234");
FoxPlugin.addParameter(PARAM_CURRENCY,  "USD");
FoxPlugin.addParameter(PARAM_PRICE, "20");
FoxPlugin.addParameter(“my_param”, "ABC");
FoxPlugin.sendLtv(70, "Taro");
```