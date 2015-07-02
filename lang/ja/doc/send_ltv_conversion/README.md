## sendLtvの詳細

sendLtvConversionメソッドを利用することで、広告流入別の課金金額や入会数などを計測することができます。計測のために、任意の地点にLTV成果通信を行うコードを追加します。

ソースの編集は、成果が上がった後に実行されるスクリプトに処理を記述します。例えば、会員登録やアプリ内課金後の課金計測では、登録・課金処理実行後のコールバック内にLTV計測処理を記述します。
対象のスクリプト（C#、またはJavaScript）によって編集内容が異なりますのでご注意ください。

成果がアプリ内部で発生する場合、成果処理部に以下C#で記述します。
JavaScriptに編集する場合は、文中の「FoxPlugin」を「FoxPluginJS」に読み替えてください。

成果通知のコードを追加

```C#
FoxPlugin.sendLtv(成果地点 ID);
```
> 成果地点ID(必須)：管理者より連絡します。その値を入力してください。


アプリ内部の成果に、広告主端末ID（会員IDなど）を含める事ができ、これを基準とした成果計測が行えます。LTV成果に広告主端末IDを付与したい場合は以下のように記述してください。

```C#
	FoxPlugin.sendLtv(成果地点ID, "広告主端末ID");
```

> 成果地点ID(必須)：管理者より連絡します。その値を入力してください。
広告主端末ID(オプション)：広告主様が管理しているユニークな識別子（会員IDなど）です。
指定できる値は64文字以内の半角英数字です。


アプリ内計測時には、パラメータをオプションとして設定する事が可能です。

```C#
	FoxPlugin.addParameter("パラメータ名", "値");
```

指定できるパラメータは次の通りです。

|パラメータ名|概要|
|:------|:------|
|PARAM_SKU|Stock Keeping Unit(商品管理コード)<br>（半角英数字32文字まで）<br>商品の在庫管理する際に使用してください|
|PARAM_PRICE|Price<br>（整数値　日本円）<br>売上額を管理する際に使用してください。|
|PARAM_CURRENCY|Currency<br>（半角英字3文字の通貨コード）<br>通貨別で課金額を集計する際に使用してください。<br>通貨が設定されていない場合、PriceをJPY(日本円)として扱います。|
|任意でパラメータを加える事も可能です。|FoxPlugin.addParameter(“パラメータ名”, “値”);<br>※1 同一パラメータ名を記述した場合は、後者が有効となります。<br>※2 アンダースコア（"_"）をパラメータ名の先頭に記述しないでください。<br>※3 半角英数字以外は使用できません。|

PARAM_CURRENCYには[ISO 4217](http://ja.wikipedia.org/wiki/ISO_4217)で定義された通貨コードを指定してください。

設定例：
```C#
FoxPlugin.addParameter(PARAM_SKU, "ABC1234");
FoxPlugin.addParameter(PARAM_CURRENCY,  "USD");
FoxPlugin.addParameter(PARAM_PRICE, "20");
FoxPlugin.addParameter(“my_param”, "ABC");
FoxPlugin.sendLtv(70, "Taro");
```
