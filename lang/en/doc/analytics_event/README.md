## Event tracking based on app access tracking

Using the app access tracking feature, it is possible to track ad-driven (per ad campaign) and non-ad driven events or item purchases. To implement this tracking, use the sendEvent() method as shown below.

To track events such as the completion of a tutorial, or new member registration, call the method as shown below.

```cs
FoxPlugin.sendEvent(eventName, action, label, quantity);
```

To track an item purchase, call the sendEventPurchase() method as shown below.

```cs
FoxPlugin.sendEventPurchase(eventName, action, label, orderId, sku, itemName, price, quantity, currency);
```

Specification of the parameters of the sendEvent method is as follows.

|Parameter|Data type|maximum length|Summary|
|:------|:------:|:------:|:------|
|eventName|String|255| Identifier for the event.|
|action|String|255|Action associated to the event. Null acceptable.|
|label|String|255|Label associated to the event. Null acceptable.|
|orderId|String|255|Order id. Null acceptable.|
|sku|String|255|Product/item code. Null acceptable.|
|itemName|String|255|Product/item name. Pass empty string ("") if not applicable..|
|price|double||Product/item price.|
|quantity|int||Amount of the product/item bought. Sale is calculated as price*quantity. |
|currency|String||Currency code. "JPY" is used by default if null is passed.|

> Specify the currency code using the [`ISO 4217`] standard (http://ja.wikipedia.org/wiki/ISO_4217) for currency.

If you want to use item purchase event as an LTV conversion point too, then implement LTV measurement in the same place as event tracking.

Here is the sample code that tracks an $3 item purchase event for LTV measurement.


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
