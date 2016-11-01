# The detail of sendLtv

By using sendLtvConversion method, it is able to measure billing amount of each advertisement inflow and the total number of admission. For measurements, add the code of LTV result communication at arbitrary point.

For edition of source, note the management to scripts conducted after acquisition of result. For example, for the charging measurement after member registration and billing in application, note LTV measurement management in callback after management of registration and billing. Please be careful that the content of edition is changed depending on script (C#, or JavaScript）.

In the case that result is generated in application, note following C# to result management department. In the case of editing to JavaScript, change from「FoxPlugin」to「FoxPluginJS」in sentences.

Add the code of result notification

```cs
FoxPlugin.sendLtv(LTV point ID);
```
> LTV point ID(Mandatory)：There is a contact from admistrater, so please type the value.

It is able to include advertiser terminal ID (such as member ID)in result in application and measure result measurement based on this. In the case of giving advertisement terminal ID to LTV result, please note like following.

```cs
	FoxPlugin.sendLtv(LTV point ID, "Advertisement terminal ID");
```

> Result point ID(mandatory)：There is a contact from administrator., so please type the value.
Advertsiment terminal ID(Option)：It is unique identifier (such as member ID) administered by advertisers. The specifying value is half-width alphanumeric within 64.


When measuring in application, it is able to set parameter as option.

```cs
	FoxPlugin.addParameter("Parameter name", "value");
```

Available parameters are noted below.

|Parameter name|Outline|
|:------|:------|
|PARAM_SKU|Stock Keeping Unit(Product administration code)<br>（Till 32 characters in hal-width alphanumeric）<br>Please use when controlling products in stocks.|
|PARAM_PRICE|Price<br>（Integar value Japanese yen）<br>Please use when contrlling amounts of sales.|
|PARAM_CURRENCY|Currency<br>（Currency code of three half-width alphabet）<br>Please use when aggregating total amouunt in each currency.<br>In the case of not specifying the currency, Price will be JPY(Japanese Yen).|
|It is able to arbitrary add parameter.|FoxPlugin.addParameter(“Paramter name”, “value”);<br>※1  In the case of same parameter name, the latter is available.<br>※2 Please do not note underscore（"_"）at start of parameter name.<br>※3 Only hald-width alphanumeric is available.|

lease specify the currency code defined by [`ISO 4217`](http://ja.wikipedia.org/wiki/ISO_4217) for PARAM_CURRENCY.

Setting example :
```cs
FoxPlugin.addParameter(PARAM_SKU, "ABC1234");
FoxPlugin.addParameter(PARAM_CURRENCY,  "USD");
FoxPlugin.addParameter(PARAM_PRICE, "20");
FoxPlugin.addParameter(“my_param”, "ABC");
FoxPlugin.sendLtv(70, "John");
```


---
[TOP](/lang/en/README.md)
