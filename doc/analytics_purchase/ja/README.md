## アクセス解析による課金計測

アクセス解析機能を利用し、自然流入経由を含めた広告別の課金計測を行うことができます。アクセス解析による課金計測を行うために、次のsendEventメソッドを実装します。

```C#
FoxPlugin.sendEventPurchase(eventName, action, label, orderId, sku, itemName, price, quantity, currency);
```

sendEventメソッドのパラメータの仕様は下記の通りです。

|パラメータ|型|最大長|概要|
|:------|:------:|:------:|:------|
|eventName|String|255|トラッキングを行うイベントを識別できる任意の名前を設定します。イベント名は自由に設定可能です。|
|action|String|255|イベントに属するアクション名を設定します。アクション名は自由に設定可能です。特に指定がない場合はnullでも構いません。|
|label|String|255|アクションに属するラベル名を設定します。ラベル名は自由に設定可能です。特に指定がない場合はnullでも構いません。|
|orderId|String|255|注文番号等を指定します。特に指定がない場合はnullでも構いません。|
|sku|String|255|商品コード等を指定します。特に指定がない場合はnullでも構いません。|
|itemName|String|255|商品名を指定します。指定がない場合は空文字("")を指定してください。|
|price|double||商品単価を指定します。|
|quantity|int||数量を指定します。price * quantityが売上金額として計上されます。|
|currency|String||通貨コードを指定します。nullの場合は"JPY"が指定されます。|

> currencyには[ISO 4217](http://ja.wikipedia.org/wiki/ISO_4217)で定義された通貨コードを指定してください。

LTV計測においても課金を成果地点としている場合には、同一の箇所にLTVとアクセス解析のそれぞれの計測処理を実装します。

サンプルとして、以下にアメリカドルで300円の課金を行った場合の実装例を記載致します。



```C#		
// LTV計測による課金計測
FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "300");
FoxPlugin.sendLtv(成果地点 ID);

// アクセス解析による課金計測
FoxPlugin.sendEventPurchase("purchase", null, null, null, null, "", 300, 1, "JPY");
```
