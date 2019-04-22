# Force Operation Xとは

Force Operation X (以下F.O.X)は、スマートフォンにおける広告効果最適化のためのトータルソリューションプラットフォームです。アプリケーションのダウンロード、ウェブ上でのユーザーアクションの計測はもちろん、スマートフォンユーザーの行動特性に基づいた独自の効果計測基準の元、企業のプロモーションにおける費用対効果を最大化することができます。

本ドキュメントでは、スマートフォンアプリケーションにおける広告効果最大化のためのF.O.X SDK導入手順について説明します。

## 目次

* **[1. インストール](#install_sdk)**
  * [SDKダウンロード](https://github.com/cyber-z/public-fox-unity-sdk/releases)
  * [Unityプラグインの導入方法](./doc/integration/README.md)
   * [iOSプロジェクトの設定](./doc/integration/ios/README.md)
   * [Androidプロジェクトの設定](./doc/integration/android/README.md)
  * [最新バージョンへのマイグレーションについて](./doc/update/README.md)
* **[2. F.O.X SDKのアクティベーション](#activate_sdk)**
* **[3. インストール計測の実装](#track_install)**
	*	[インストール計測の詳細](./doc/track_install/README.md)
* **[4. リエンゲージメント計測の実装](#track_reengagement)**
* **[5. アプリ内イベントの計測](#track_event)**
	* [セッション(起動)イベントの計測](#track_event)
	* [その他アプリ内イベントの計測](#track_other_event)
	* [イベント計測の詳細](./doc/track_event/README.md)
* **[6. 最後に必ずご確認ください](#trouble_shooting)**

---

## F.O.X SDKとは

F.O.X SDKをアプリケーションに導入することで、以下の機能を実現します。

* **インストール計測**

広告流入別にインストール数を計測することができます。

* **LTV計測**

流入元広告別にLife Time Valueを計測します。主な成果地点としては、会員登録、チュートリアル突破、課金などがあります。各広告別に登録率、課金率や課金額などを計測することができます。

* **アクセス解析**

自然流入と広告流入のインストール比較。アプリケーションの起動数やユニークユーザー数(DAU/MAU)。継続率等を計測することができます。


<div id="install_sdk"></div>

## 1. インストール

以下のページより最新のSDKをダウンロードしてください。

[SDKリリースページ](https://github.com/cyber-z/public-fox-unity-sdk/releases)

既にアプリケーションにSDKが導入されている場合には、[最新バージョンへのマイグレーションについて](./doc/update/README.md)をご参照ください。

ダウンロードしたSDK「FOX_UnityPlugin_<version>.zip」を展開し、アプリケーションのプロジェクトに組み込んでください。

[Unityプラグインの導入方法](./doc/integration/README.md)

### 各OS毎の設定

* [iOSプロジェクトの設定](./doc/integration/ios/README.md)
* [Androidプロジェクトの設定](./doc/integration/android/README.md)

<div id="activate_sdk"></div>

## 2. F.O.X SDKのアクティベーション

F.O.X SDKのアクティベーションを行うため、アプリの起動時点に以下の実装を行います。<br>
FoxConfigに必須事項を格納したら`Fox.activate`を実行します。

```cs
using Cyz;
...

void Start() {
	FoxConfig config = new FoxConfig ();
	config.iOSAppId = 発行されたiOS用APP_ID;
	config.iOSAppKey = 発行されたiOS用APP_KEY;
	config.iOSAppSalt = 発行されたiOS用APP_SALT;
	config.androidAppId = 発行されたAndroid用APP_ID;
	config.androidAppKey = 発行されたAndroid用APP_KEY;
	config.androidAppSalt = 発行されたAndroid用APP_SALT;

	// オプショナル設定
	config.iOSCustomizedUserAgentSupport = true; // User-Agentをカスタマイズしたい場合trueにする
	if(debug) config.isDebug = true;
	Fox.activate(config);

	// User-Agentのカスタマイズの処理を行う
}
```
> ※ appId、salt、appKeyの値については、アプリ登録後、F.O.X管理画面のアプリ一覧>該当アプリ右上の設定ボタン>SDK導入をご確認ください。

> ※ `isDebug`はtrueにするとデバッグ用ログを出力することが可能となります。

> ※ User-Agentをカスタマイズする場合、必ずiOSCustomizedUserAgentSupportをtrueにし、Fox.activateメソッドをコールした後でUser-Agentのカスタマイズをおこなってください。

### 2.1 オフラインモード
F.O.X SDKの計測機能を停止しトラッキングを無効化する設定です。

オフラインモードを有効にする場合は、config.isOfflineにtrueを、無効にする場合はfalseを設定してください（未設定の場合、オフラインモードは無効のままです）。

- 開発期間などでF.O.Xへデータを送信したくない場合や、配信地域によって計測を停止したい場合などで本機能をご利用ください。
- ユーザ許諾をもとにオフラインモードの有効無効を設定したい場合、ユーザ許諾後にactivate()を実行してください(activate()はアプリ起動時に常に呼び出し必要となります)
- 自動計測ではオフラインモードの設定は非対応となります。手動計測での実装を行ってください。
- オフラインモードを設定した場合、アプリをアンイストールするまで設定は反映されます。

```cs
{
  ...
  // オフラインモードの設定
  config.isOffline = true;
  Fox.activate(config);
}
```

<div id="track_install"></div>

## 3. インストール計測の実装

初回起動のインストール計測を実装することで、広告の効果測定を行うことができます。

### インストール計測の実装

インストール計測を行うには、起動時に実行されるスクリプトから`Fox.trackInstall`をコールします。

```cs
using Cyz;
...

	Fox.trackInstall();
```

*	[インストール計測のオプション機能](./doc/track_install/README.md)

<div id="track_reengagement"></div>

## 4. リエンゲージメント計測の実装

メソッドの実行やF.O.XのAPIのコールは不要です。  
  
iOSの場合は、プラグインを導入するのみでリエンゲージメント計測が可能です。詳細はUnityプラグインの導入方法をご参照ください。  
[Unityプラグインの導入方法](./doc/integration/README.md)  
Androidの場合は、Androidプロジェクトの設定をご参照ください。  
[Androidプロジェクトの設定](./doc/integration/android/README.md#リエンゲージメント計測の実装)

<div id="track_event"></div>

## 5. アプリ内イベントの計測

起動セッション、リエンゲージメント、会員登録、チュートリアル突破、課金など任意の成果地点にイベント計測を実装することで、流入元広告のLTVや継続率を測定することができます。それらの計測が不要の場合には、各項目の実装を省略できます。

<div id="track_session"></div>

### セッション(起動)イベントの計測

自然流入と広告流入のインストール数比較、アプリケーションの起動数やユニークユーザー数(DAU/MAU)、継続率等を計測することができます。アクセス解析が不要の場合には、本項目の実装を省略できます。
<br>
アプリケーションが起動、もしくはバックグラウンドから復帰する際にセッション計測を行うコードを追加します。不要の場合には、本項目の実装を省略できます。

```cs
using Cyz;
...

	Fox.trackSession();
```

<div id="track_other_event"></div>

### その他アプリ内イベントの計測

会員登録、チュートリアル突破、課金など任意の成果地点にイベント計測を実装することで、流入元広告のLTVを測定することができます。<br>
イベント計測が不要の場合には、本項目の実装を省略できます。<br>
成果がアプリ内部で発生する場合、成果処理部に以下のように記述してください。<br>

**[チュートリアルイベントの計測例]**
```cs
using Cyz;
...
        // LTV計測必要な場合
	int ltvId = 成果地点ID;
	FoxEvent e = new FoxEvent("_tutorial_comp", ltvId);
	e.buid = "USER_001"
	Fox.trackEvent(e);
```
```cs
using Cyz;
...
        // イベント計測のみの場合
	FoxEvent e = new FoxEvent("_tutorial_comp");
	e.buid = "USER_001"
	Fox.trackEvent(e);
```

> LTV計測を行うためには、各成果地点を識別する`成果地点ID`を指定する必要があります。  
> イベント計測のみを計測する場合、イベント名だけ入力してください。

**[課金イベントの計測例]**

課金計測を行う場合には、課金が完了した箇所で以下のように課金額を指定してください。

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

> currencyの指定には[ISO 4217](http://ja.wikipedia.org/wiki/ISO_4217)で定義された通過コードを指定してください。

* [イベント計測の詳細](./doc/track_event/README.md)

<div id="trouble_shooting"></div>

## 6. 最後に必ずご確認ください（これまで発生したトラブル集）

### 6.1. URLスキームの設定がされずリリースされたためブラウザからアプリに遷移ができない

Cookie計測を行うために外部ブラウザを起動した後に、元の画面に戻すためにはURLスキームを利用してアプリケーションに遷移させる必要があります。この際、独自のURLスキームが設定されている必要があり、URLスキームを設定せずにリリースした場合にはこのような遷移を行うことができなくなります。

### 6.2. URLスキームに大文字や記号が含まれ、正常にアプリに遷移されない

環境によって、URLスキームの大文字小文字が判別されないことにより正常にURLスキームの遷移が行えない場合があります。URLスキームは全て小文字の英数字で設定を行ってください。


### 6.3. URLスキームの設定が他社製アプリと同一でブラウザからそちらのアプリが起動してしまう

iOSにおいて、複数のアプリに同一のURLスキームが設定されていた場合に、どのアプリが起動するかは不定です。確実に特定のアプリを起動することができなくなるため、URLスキームは他社製アプリとはユニークになるようある程度の複雑性のあるものを設定してください。

### 6.4. 短時間で大量のユーザー獲得を行うプロモーションを実施したら正常に計測がされなかった

iOSには、アプリ起動時に一定時間以上メインスレッドがブロックされるとアプリケーションを強制終了する仕様があります。起動時の初期化処理など、メインスレッド上でサーバーへの同期通信を行わないようにご注意ください。リワード広告などの大量のユーザーを短時間で獲得した結果、サーバーへのアクセスが集中し、通信のレスポンスが非常に悪くなることでアプリケーションの起動に時間がかかり、起動時に強制終了され正常に広告成果が計測できなくなった事例がございます。

以下の手順で、こうした状況をテストすることができますので、以下の設定でアプリケーションが正常に起動するかをご確認ください。

`iOS「設定」→「デベロッパー」→「NETWORK LINK CONDITIONER」`

* 「Enable」をオン
* 「Very Bad Network」をチェック


### 6.5. F.O.Xで確認できるインストール数の値がGoogle Play Developer Consoleの数字より大きい

F.O.Xではいくつかの方式を組み合わせて端末の重複インストール検知を行っています。
重複検知が行えない設定では、同一端末でも再インストールされる度にF.O.Xは新規のインストールと判定してしまいます。

重複検知の精度を向上するために、以下の設定を行ってください。

* [広告IDを利用するためのGoogle Play Services SDKの導入](./doc/integration/android/google_play_services/README.md)

* [（オプション）外部ストレージを利用した重複排除設定](/lang/ja/doc/integration/android/external_storage/README.md)

* [（オプション）Android M オートバックアップ機能の利用](./doc/integration/android/auto_backup/README.md)

---
[トップメニュー](/README.md)
