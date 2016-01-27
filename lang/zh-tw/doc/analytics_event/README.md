## 依靠流量分析進行消費計測
能夠計測按不同廣告流入和自然流入的用戶進行的Event和銷售額。
為了進行依據流量分析的計測，請安裝下面的sendEvent方法。

如果做教程突破或會員登錄這樣的Event計測，請按下面那樣來書寫。

```cs
FoxPlugin.sendEvent(eventName, action, label, quantity);
```

如果做消費計測，請按下面那樣來書寫。

```cs
FoxPlugin.sendEventPurchase(eventName, action, label, orderId, sku, itemName, price, quantity, currency);
```

sendEvent方法的參數說明如下。

|參數|類型|最大長度|概要|
|:------|:------:|:------:|:------|
|eventName|String|255|設定能夠識別監測Event的任意名稱。可以自由設定。|
|action|String|255|設定屬於Event的Action名。可以自由設定。不做特別指定的場合可以為null。|
|label|String|255|屬於Action的Label名。可以自由設定。不做特別指定的場合可以為null。|
|orderId|String|255|指定訂單號。不做特別指定的場合可以為null。|
|sku|String|255|指定商品代號sku。不做特別指定的場合可以為null。|
|itemName|String|255|指定商品名。不指定時請設定為空字符串("")。|
|price|double||指定商品單價。|
|quantity|int||指定數量。按price * quantity的銷售額來計算在內。|
|currency|String||指定貨幣代碼。null的場合默認指定為"JPY"。|

> 在currency裡請按[ISO 4217](http://ja.wikipedia.org/wiki/ISO_4217)定義的貨幣代碼來指定。

如果希望在LTV計測地點也做消費計測，請在同一個地點安裝LTV和流量分析的各自的計測處理代碼。

下面是一個計測消費3美元的安裝實例。

```cs
// 依靠LTV計測進行消費計測
FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");
FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "3");
FoxPlugin.sendLtv(成果地点 ID);

// 依靠流量分析進行消費計測
FoxPlugin.sendEventPurchase("purchase", null, null, null, null, "", 3, 1, "USD");
```

---
[TOP](/lang/zh-tw/README.md)
