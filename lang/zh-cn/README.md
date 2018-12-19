# 什么是Force Operation X

Force Operation X (下面简称F.O.X)是一款基于智能手机的，用来最大改善广告效果的综合解决方案平台。除了对APP下载量和网络用户操作的基本计测外，还能基于手机用户行为特性采用独自效果计测基準，实现了企业宣传推广时费用与效果比的最大改善。

在这个文档里，详细讲解了基于智能手机平台优化广告效果的F.O.X SDK的导入步骤。

## 目录

* **[1. 导入](#install_sdk)**
  * [SDK导入](https://github.com/cyber-z/public-fox-unity-sdk/releases)
  * [Unity插件导入步骤](./doc/integration/README.md)
   * [iOS项目设置](./doc/integration/ios/README.md)
   * [Android项目设置](./doc/integration/android/README.md)
  * [最新版本更新](./doc/update/README.md)
* **[2. F.O.X SDK激活](#activate_sdk)**
* **[3. 执行Install计测](#track_install)**
	*	[Install计测详细](./doc/track_install/README.md)
* **[4. 执行流失唤回广告计测](#track_reengagement)**
* **[5. APP内事件计测](#track_event)**
	* [session(启动)事件计测](#track_event)
	* [其他APP内事件计测](#track_other_event)
	* [事件计测详细](./doc/track_event/README.md)
* **[6. 最后的注意事项](#trouble_shooting)**

---

## 什么是F.O.X SDK

将F.O.X SDK导入APP之后，能够实现以下功能。

* **Install计测**

能够计测不同广告带来的安装数。

* **LTV计测**

可以计测不同广告来源的Life Time Value。主要的成果地点为会员注册、新手引导完成、付费等。能够分别监测各广告的登录率、付费率以及付费金额。

* **流量分析**

比较自然流量和广告流量带来的安装。能够计测App的启动次数和unique用户数(DAU/MAU)、留存率等。


<div id="install_sdk"></div>

## 1. 安装

请从以下页面中下载最新安定版(Latest release)SDK。

[SDK发布页面](https://github.com/cyber-z/public-fox-unity-sdk/releases)

APP中已经导入SDK的场合，请参考[更新到最新版本](./doc/update/README.md)。

下载并展开SDK「FOX_UnityPlugin_<version>.zip」，安装至APP项目中。

[Unity插件的导入方法](./doc/integration/README.md)

### 根据OS设置

* [iOS项目设置](./doc/integration/ios/README.md)
* [Android项目设置](./doc/integration/android/README.md)

<div id="activate_sdk"></div>

## 2. F.O.X SDK激活

为激活F.O.X SDK，需在APP启动时执行以下代码。<br>
FoxConfig中执行收纳了必须事项的`Fox.activate`。

```cs
using Cyz;
...

void Start() {
    FoxConfig config = new FoxConfig ();
    config.iOSAppId = 发行的iOS APP_ID;
    config.iOSAppKey = 发行的iOS APP_KEY;
    config.iOSAppSalt = 发行的iOS APP_SALT;
    config.androidAppId = 发行的Android APP_ID;
    config.androidAppKey = 发行的Android APP_KEY;
    config.androidAppSalt = 发行的Android APP_SALT;

    // 设定optional
    config.iOSCustomizedUserAgentSupport = true; // 定制化User-Agent的时候设定为true
    if(debug) config.isDebug = true;
    Fox.activate(config);

    // 进行User-Agent的定制化处理
}
```
> ※ APP登录以后，可以在F.O.X管理画面的【APP一览>此APP右上方的设定按钮>导入SDK】的地方来确认appId、salt、appKey的值。

> ※ `isDebug`为true时，能够输出调试日志。

> ※ 定制化User-Agent的时候、请务必将iOSCustomizedUserAgentSupport设置为true、在调用Fox.activate方法以后，执行User-Agent定制化处理。

### 2.1 线下模式
开启线下模式功能，可停止F.O.X SDK的所有监测行为。

将config.isOffline设定为true来开启线下模式，设定为false来关闭线下模式（未设定时默认为关闭线下模式）。

- 在开发期间，如不想将数据发送到F.O.X，或者希望按照投放地域停止计测的时候，可以利用这个功能。
- 根据用户许可来设定线下模式是否开启时，请确保在用户许可之后执行activate()。(activate()需要在App每次启动时去执行)
- 自动计测的代码实装方式不适用于线下模式。请按手动计测来实装代码。
- 设定将保持生效至App被删除。

```cs
{
  ...
  // 线下模式的设定
  config.isOffline = true;
  Fox.activate(config);
}
```

<div id="track_install"></div>

## 3. 执行Install计测

进行首次启动的Install计测，可以计测广告效果。

### 执行Install计测

为进行Install计测，从启动时执行的脚本中呼出`Fox.trackInstall`。

```cs
using Cyz;
...

	Fox.trackInstall();
```

*	[Install计测详细](./doc/track_install/README.md)

<div id="track_reengagement"></div>

## 4. 执行流失唤回广告计测

唤回计测无需调用函数。  
  
iOS在Unity导入插件时已默认开启唤回计测。详细可参考以下文档。  
[Unity插件导入步骤](./doc/integration/README.md)  
Android的设置方法请参考以下文档。  
[Android项目设置](./doc/integration/android/README.md#track_reengagement)

<div id="track_event"></div>

## 5. APP内事件计测

启动session、会员注册、新手引导突破、付费等任意成果地点中进行事件计测，可以计测广告渠道的LTV和留存率，如不需要以上结果，可以略过。

<div id="track_session"></div>

### session（启动）事件计测

可以计测自然流量和广告流量的安装数对比、APP启动次数和unique用户人数（DAU/MAU)、留存率等。如不需要流量分析，可以忽略本项。
<br>
APP启动时或后台恢复时添加session计测代码。无需该计测时，可以忽略本项。

```cs
using Cyz;
...

	Fox.trackSession();
```

<div id="track_other_event"></div>

### 其他的APP内事件计测

在会员注册，新手引导完成，付费等任意成果地点执行事件计测，能够测定广告流入源的LTV。<br>
无需事件计测时，可以忽略本项。<br>
成果在APP内部产生的情况，请在处理成果的代码部分进行以下描述。<br>

**[新手引导事件计测案例]**
```cs
using Cyz;
...
        // 需要LTV计测时
	int ltvId = 成果地点ID;
	FoxEvent e = new FoxEvent("_tutorial_comp", ltvId);
	e.buid = "USER_001"
	Fox.trackEvent(e);
```
```cs
using Cyz;
...
        // 只进行事件计测时
	FoxEvent e = new FoxEvent("_tutorial_comp");
	e.buid = "USER_001"
	Fox.trackEvent(e);
```

> 进行LTV计测时，需指定识别成果地点的`成果地点ID`。  
> 只进行事件计测时，只需输入事件名。

**[付费事件计测案例]**

进行付费计测时，请在付费完成的位置指定付费金额。

```cs
using Cyz;
...

	int ltvId = 成果地点ID;
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

> currency请指定[ISO 4217](http://ja.wikipedia.org/wiki/ISO_4217)里定义的货币代码。

* [事件计测详细](./doc/track_event/README.md)

<div id="trouble_shooting"></div>

## 6. 最后需确认内容（常见问题集）

### 6.1. 未设置URL SCHEME 进行发布时无法从浏览器跳转至APP

进行Cookie计测时启动浏览器以后，必须使用URL scheme跳转回到APP画面。此时需要设置URL scheme，未设置scheme就上线发布时会导致无法正常迁移。

### 6.2. URL SCHEME中含有大写字母时，无法正常跳转APP。

根据运行环境，会出现因为URL SCHEME 的大小写字母不能判定而导致URL SCHEME 无法正常迁移的情况。请将URL SCHEME 全部设置为小写英文或数字或小数点。

### 6.3. URL scheme设置与其他公司APP相同时，浏览器会跳转其他APP

iOS中，多个APP设置为同一个URL scheme时，会随机启动APP。由于可能导致无法启动指定的APP，请将URL scheme区别与其他APP来设定。


### 6.4. 进行短时间内获取大量用户的推广时无法正确计测

iOS中，APP启动时超过一定时间主线程被阻止运行时，会强制关闭APP。请注意不要让启动时的初始化处理在主线程上与服务器同时进行通讯。短时间内获得大量用户的激励广告等会因为集中访问服务器，通讯回复较差而导致APP启动时间延长或强制关闭等情况，从而导致无法正确计测广告结果。

按照以下步骤可以进行以上情况的测试，请进行以下设置，确认APP是否正常启动。

`iOS「设置」→「属性」→「NETWORK LINK CONDITIONER」`

* 「Enable」设置为on
* 勾选「Very Bad Network」


### 6.5. F.O.X中安装数的值会大于Google Play Developer Console的数值

F.O.X结合多种方式来进行终端重复安装的检查。
当设置无法进行检查重复时，同一终端的再次安装可能会被F.O.X判定为新的安装。

为提高排查重复的精度，请进行下面的设置。


* [导入Google Play Services SDK来使用广告ID
](./doc/integration/android/google_play_services/README.md)

* [（任意）利用外部储存优化重复排除](/lang/ja/doc/integration/android/external_storage/README.md)

* [（任意）使用自动备份功能 Android M ](./doc/integration/android/auto_backup/README.md)

---
[Top](/README.md)
