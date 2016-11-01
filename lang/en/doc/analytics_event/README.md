## Event measurement by access analysis

Using the access analysis function, it is possible to measure respectively events and sales including via natural influx advertising.  To conduct event measurement and billing measurement by access analysis, implement next sendEvent method.

In the case of event measurement such as completion of tutorial and member registration, please note like below.

```cs
FoxPlugin.sendEvent(eventName, action, label, quantity);
```

In the case of billing measurement, please note like below.

```cs
FoxPlugin.sendEventPurchase(eventName, action, label, orderId, sku, itemName, price, quantity, currency);
```

Specification of the parameters of the sendEvent method is as follows.

|Parameter|type|maximum length|outline|
|:------|:------:|:------:|:------|
|eventName|String|255| Set the arbitrary name identifying event conducting tracking. It is able to set event name freely.|
|action|String|255|Set the action name belonging to event. It is able to set action name freely. If not specifying, null is acceptable.|
|label|String|255|Set the label name belonging to action. It is able to set label name freely. If not specifying, null is acceptable.|
|orderId|String|255|Specify the order number. If not specifying, null is acceptable.|
|sku|String|255|Specify the product code. If not specifying, null is acceptable.|
|itemName|String|255|Specify the product name. If not specifying, specify the empty character which is ("").|
|price|double||Specify the unit price|
|quantity|int||SPecify the amount. price * quantity is calculated as total sales.|
|currency|String||Specify the currency code In the case of null, "JPY" is specified.|

> Specify the currency code defined by [`ISO 4217`](http://ja.wikipedia.org/wiki/ISO_4217) for currency.

In the case of setting billing as result points in LTV measurement, implement measurement management of each LTV and access analysis at same place.

 As a sample, the example of implementation is noted below and it is an example of 3 American dollars.



```cs
// Billing measurement by LTV measurement
FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");
FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "3");
FoxPlugin.sendLtv(LTV point ID);

// billing measurement by access analysis
FoxPlugin.sendEventPurchase("purchase", null, null, null, null, "", 3, 1, "USD");
```

---
[TOP](/lang/en/README.md)
