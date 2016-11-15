# More on sendLtv()

Using sendLtvConversion() method, it is possible to track item sales and member registrations driven by each ad campaign. To perform this tracking, call the sendLtv() method wherever required.

Call sendLtv() in a script that runs after the event to be tracked has been successfully completed. For example, to track new member registration or an in-app item purchase, call sendLtv() in the callback after successful execution of member registration or in-app item purchase code. Please note that the implementation details are different for each script (C# and JavaScript).

If the event is executed wholly inside the app, include the C# code shown below in the part that runs after the successful completion of the event. In case of JavaScript, replace 'FoxPlugin' with 'FoxPluginJS'.

Code to send LTV conversion.

```cs
FoxPlugin.sendLtv(LTV conversion point ID);
```
> LTV conversion point ID(Required)： This value will be informed to you by a FOX administrator.

It is possible to pass the user's advertising ID (membership ID etc.) as the second argument to sendLtv() making it possible to perform the measurement per user. This can be done as shown below.

```cs
	FoxPlugin.sendLtv(LTV conversion point ID, "advertising ID");
```

> LTV conversion point ID(Required)： This value will be informed to you by a FOX administrator.
> advertising ID: ID used by the advertiser to uniquely identify the user's device (membership ID etc). Should be less than or equal to 64 alphanumeric characters long.

To enable parameterized tracking, pass the parameter name and value as shown below.

```cs
	FoxPlugin.addParameter("Parameter name", "value");
```

The list of available parameters is shown below.

|Parameter name|Summary|
|:------|:------|
|PARAM_SKU|Stock Keeping Unit(Product management code)<br>（32 characters alphanumeric）<br>Please use this when controlling the item stock.|
|PARAM_PRICE|Price<br>（Integer value, Japanese yen）<br>Please use when controlling the amount of sales.|
|PARAM_CURRENCY|Currency<br>（Three characters long currency code）<br>Please use when aggregating total amount in different currencies.<br>If not specified, JPY (Japanese Yen) will be used by default.|
|Arbitrary parameter|FoxPlugin.addParameter(“Paramter name”, “value”);<br>※1  If the same parameter is specified more than once, the latter value will be given priority.<br>※2 Please don't prefix the parameter name with an underscore ('_') sign.<br>※3 Only alphanumeric characters allowed.|

Please specify the currency code as defined in [`ISO 4217`](http://ja.wikipedia.org/wiki/ISO_4217) for PARAM_CURRENCY parameter.

Example :
```cs
FoxPlugin.addParameter(PARAM_SKU, "ABC1234");
FoxPlugin.addParameter(PARAM_CURRENCY,  "USD");
FoxPlugin.addParameter(PARAM_PRICE, "20");
FoxPlugin.addParameter(“my_param”, "ABC");
FoxPlugin.sendLtv(70, "John");
```


---
[TOP](/lang/en/README.md)
