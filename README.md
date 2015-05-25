# Force Opetaion Xとは

Force Operation X (以下F.O.X)は、スマートフォンにおける広告効果最適化のためのトータルソリューションプラットフォームです。アプリケーションのダウンロード、ウェブ上でのユーザーアクションの計測はもちろん、スマートフォンユーザーの行動特性に基づいた独自の効果計測基準の元、企業のプロモーションにおける費用対効果を最大化することができます。

本ドキュメントでは、スマートフォンアプリケーションにおける広告効果最大化のためのF.O.X SDK導入手順について説明します。

## F.O.X SDKとは

F.O.X SDKをアプリケーションに導入することで、以下の機能を実現します。

* **インストール計測**

広告流入別にインストール数を計測することができます。

* **LTV計測**

流入元広告別にLife Time Valueを計測します。主な成果地点としては、会員登録、チュートリアル突破、課金などがあります。各広告別に登録率、課金率や課金額などを計測することができます。

* **アクセス解析**

自然流入と広告流入のインストール比較。アプリケーションの起動数やユニークユーザー数(DAU/MAU)。継続率等を計測することができます。

* **プッシュ通知**

F.O.Xで計測された情報を使い、ユーザーに対してプッシュ通知を行うことができます。例えば、特定の広告から流入したユーザーに対してメッセージを送ることができます。

## 1. インストール

以下のページより最新のSDKをダウンロードしてください。

[SDKリリースページ](https://github.com/cyber-z/public_fox_unity_sdk/releases)

既にアプリケーションにSDKが導入されている場合には、[最新バージョンへのアップデートについて](./doc/update/ja)をご参照ください。

ダウンロードしたSDK「FOX_UnityPlugin_<version>.zip」を展開し、アプリケーションのプロジェクトに組み込んでください。

[Unityプラグインの導入方法](./doc/integration/ja/)


## 2. インストール計測の実装

初回起動のインストール計測を実装することで、広告の効果測定を行うことができます。下記のいずれかの手順で実装を行ってください。

* GUI(Inspector)でインストール計測を設定
* コードを記述する


### GUI(Inspector)でのインストール計測の実装

アプリ起動時に一度だけ読み込まれるオブジェクトがある場合は、GUI(Inspector)での編集が可能です。

例）Main CameraのInspectorを利用し、インストール計測を行う

1. プロジェクトの「Plugins/FoxPlugin.cs」をMain Cameraにドラッグ＆ドロップする
2. Main CameraのInspector上で、Fox PluginスクリプトのUrl変数に対してdefaultという文字列を指定する


### コードを記述する場合のインストール計測の実装

GUI(Inspector)を利用せず、スクリプトでインストール計測処理の記述を行う場合には、起動時に実行されるスクリプトからFoxPlugin.sendConversionをコールします。

```C#
FoxPlugin.sendConversion("default");
```

sendConversionの引数には、通常は上記の通り"default"という文字列を入力してください。

特定のURLヘ遷移させたい場合や、アプリケーションで動的にURLを生成したい場合には、URLの文字列を設定してください。

```C#
FoxPlugin.sendConversion("http://yourhost.com/yourpage.html");
```

## 3. LTV計測の実装

会員登録、チュートリアル突破、課金など任意の成果地点にLTV計測を実装することで、流入元広告のLTVを測定することができます。LTV計測が不要の場合には、本項目の実装を省略できます。

ソースの編集は、成果が上がった後に実行されるスクリプトに処理を記述します。例えば、会員登録やアプリ内課金後の
課金計測では、登録・課金処理実行後のコールバック内に LTV 計測処理を記述します。 対象のスクリプト(C#、または JavaScript)によって編集内容が異なりますのてこ注意ください。


```C#
FoxPlugin.sendLtv(成果地点 ID);
```

LTV計測を行うためには、各成果地点を識別する成果地点IDを指定する必要があります。sendLtvの引数に発行されたIDを指定してください。

課金計測を行う場合には、課金が完了した箇所で以下のように課金額を指定してください。

```C#
// ...
FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");
FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "20");
FoxPlugin.sendLtv(成果地点 ID);
```

> Javascriptで編集する場合は、文中の「FoxPlugin」を「FoxPluginJS」に読み替えてください。


## 4. アクセス解析の実装

自然流入と広告流入のインストール数比較、アプリケーションの起動数やユニークユーザー数(DAU/MAU)、継続率等を計測することができます。アクセス解析が不要の場合には、本項目の実装を省略できます。

アクセス解析による計測を行う場合は、下記のいずれかの手順で実装を行ってください。

* スクリプトを利用する
* コードを記述する

### スクリプトを利用する場合

「Plugins/FoxAnalyticsSession」を Main Camera にドラッグ&ドロップします。
アプリの起動時やバックグラウンドからの復帰時にセッション開始計測を行います。

> プロジェクト内に複数のSceneが存在する場合は、計測地点は全てのMain Cameraに設定してください。設定されていないSceneが表示されている状態でバックグラウンドか復帰した場合には、正確な計測が行えなくなります。


### コードを記述する場合

アプリの起動地点にて次のメソッドを実装してください。

```C#
FoxPlugin.sendStartSession();
```



* **アクセス解析による課金計測**
アクセス解析による課金計測を実施したい場合は下記のリンクを参照してください。

[アクセス解析によるイベント計測](./doc/analytics_event/ja)


## 5. 各OS毎の設定

* **iOS用Xcodeプロジェクトの設定**

### Xcodeプロジェクトのパブリッシュ

iOS用のプロジェクトを作成するために、次の手順でXcodeプロジェクトをパブリッシュし、Xcode上で必要な設定を行います。

1. メニューの「File」>「BUild Settings…」を選択する
2. Platformの「iOS」を選択し、「Switch Platform」を押下する
3. 「Player Settings」を押下し、Inspectorでご自身の環境に合わせて設定を行う
4. 	「Build」か「Build And Run」を押下し、Xcodeプロジェクトのパブリッシュを行う


### Xcodeプロジェクトの編集

パブリッシュされたXcodeプロジェクトを開き、編集します。

* **フレームワーク設定**

次のフレームワークをプロジェクトにリンクしてください。

<table>
<tr><th>フレームワーク名</th><th>Status</th></tr>
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>iAd.framework </td><td>Required</td></tr>
<tr><td>Security.framework </td><td>Required </td></tr>
<tr><td>StoreKit.framework </td><td>Required </td></tr>
<tr><td>SystemConfiguration.framework </td><td>Required </td></tr>
</table>

> AdSupport.frameworkはiOS 6以降で追加されたフレームワークのため、アプリケーションをiOS 5以前でも動作させる(iOS Deployment Targetを5.1以下に設定する)場合にはweak linkを行うために”Optional”に設定してください。

![フレームワーク設定01](https://github.com/cyber-z/public_fox_ios_sdk/raw/master/doc/config_framework/ja/img01.png)

[フレームワーク設定の詳細](https://github.com/cyber-z/public_fox_ios_sdk/blob/master/doc/config_framework/ja/README.md)

* **SDK設定**

SDKの動作に必要な設定をplistに追加します。「AppAdForce.plist」というファイルをプロジェクトの任意の場所に作成し、次のキーと値を入力してください。

<table>
<tr>
  <th>Key</th>
  <th>Type</th>
  <th>Value</th>
</tr>
<tr>
  <td>APP_ID</td>
  <td>String</td>
  <td>Force Operation X管理者より連絡しますので、その値を入力してください。</td>
</tr>
<tr>
  <td>SERVER_URL</td>
  <td>String</td>
  <td>Force Operation X管理者より連絡しますので、その値を入力してください。</td>
</tr>
<tr>
  <td>APP_SALT</td>
  <td>String</td>
  <td>Force Operation X管理者より連絡しますので、その値を入力してください。</td>
</tr>
<tr>
  <td>APP_OPTIONS</td>
  <td>String</td>
  <td>何も入力せず、空文字の状態にしてください。</td>
</tr>
<tr>
  <td>CONVERSION_MODE</td>
  <td>String</td>
  <td>1</td>
</tr>
<tr>
  <td>ANALYTICS_APP_KEY</td>
  <td>String</td>
  <td>Force Operation X管理者より連絡しますので、その値を入力してください。<br />アクセス解析を利用しない場合は設定の必要はありません。</td>
</tr>
</table>

![フレームワーク設定01](https://github.com/cyber-z/public_fox_ios_sdk/raw/master/doc/config_plist/ja/img05.png)

[SDK設定の詳細](https://github.com/cyber-z/public_fox_ios_sdk/blob/master/doc/config_plist/ja/README.md)

[AppAdForce.plistサンプル](https://github.com/cyber-z/public_fox_ios_sdk/blob/master/doc/config_plist/AppAdForce.plist)


* **Android用プロジェクトの設定**

Android 用の設定は Unity プロジェクト上で行うことができます。Unity プロジェクトに組み込まれた
AndroidManifest.xml を編集します。プロジェクトに AndroidManifest.xml が存在しない場合は、 「Plugins/Android/AndroidManifest-sample.xml」を「AndroidManifest.xml」にリネームしてご利用ください。


### パーミッションの設定

<Manifest>タグ内に次のパーミッションの設定を追加します。

```xml:
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### メタデータの設定

SDKの実行に必要な情報を<application>タグ内に追加します。

```xml:
<meta-data android:name="APPADFORCE_APP_ID" android:value="Force Operation X管理者より連絡しますので、その値を入力してください。" />
<meta-data android:name="APPADFORCE_SERVER_URL" android:value="Force Operation X管理者より連絡しますので、その値を入力してください。" />
<meta-data android:name="APPADFORCE_CRYPTO_SALT" android:value="Force Operation X管理者より連絡しますので、その値を入力してください。" />
<meta-data android:name="ANALYTICS_APP_KEY" android:value="Force Operation X管理者より連絡しますので、その値を入力してください。" />
```

設定するキーとバリューは以下の通りです。

|パラメータ名|必須|概要|
|:------|:------|:------|
|APPADFORCE_APP_ID|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|
|APPADFORCE_SERVER_URL|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|
|APPADFORCE_CRYPTO_SALT|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|
|ANALYTICS_APP_KEY|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|


### インストールリファラ計測の設定
インストールリファラーを用いたインストール計測を行うために下記の設定を<application>タグに追加します。

```xml:
<receiver android:name="jp.appAdForce.android.InstallReceiver" android:exported="true">
	<intent-filter>
		<action android:name="com.android.vending.INSTALL_REFERRER" />
	</intent-filter>
</receiver>
```

既に"com.android.vending.INSTALL_REFERRER"に対するレシーバークラスが定義されている場合には、[二つのINSTALL_REFERRERレシーバーを共存させる場合の設定](https://github.com/cyber-z/public_fox_android_sdk/blob/master/doc/install_referrer/ja/README.md)をご参照ください。


### リエンゲージメント計測の実装

カスタムURLスキーム経由の起動を計測するために必要な設定を<application>タグ内に追記します。
カスタムURLスキームは他のActivityで設定しているものと異なる値を設定してください。

```xml
<activity android:name="jp.appAdForce.android.IntentReceiverActivity">
	<intent-filter>
		<action android:name="android.intent.action.VIEW" />
		<category android:name="android.intent.category.DEFAULT" />
		<category android:name="android.intent.category.BROWSABLE" />
		<data android:scheme="カスタム URL スキーム" />
	</intent-filter>
</activity>
```

[広告IDを利用するためのGoogle Play Services SDKの導入](./doc/google_play_services/ja/)

[（オプション）外部ストレージを利用した重複排除設定](https://github.com/cyber-z/public_fox_android_sdk/tree/master/doc/external_storage/ja)

[AndroidManifest.xmlサンプル](./doc/config_android_manifest/AndroidManifest.xml)


## 6. ProGuardを利用する場合

ProGuard を利用してアプリケーションの難読化を行う際は F.O.X SDK のメソッドが対象とならないよう、以下の設定 を追加してください。

```
-keepattributes *Annotation*

-libraryjars libs/AppAdForce.jar
-keep interface jp.appAdForce.** { *; }
-keep class jp.appAdForce.** { *; }
-keep class jp.co.dimage.** { *; }
-keep class com.google.android.gms.ads.identifier.* { *; }
-dontwarn jp.appAdForce.android.ane.AppAdForceContext
-dontwarn jp.appAdForce.android.ane.AppAdForceExtension
-dontwarn com.adobe.fre.FREContext
-dontwarn com.adobe.fre.FREExtension
-dontwarn com.adobe.fre.FREFunction
-dontwarn com.adobe.fre.FREObject
-dontwarn com.ansca.**
-dontwarn com.naef.jnlua.**
```

また、Google Play Service SDK を導入されている場合は、以下のぺージに記載されている keep 指定が記述されているかご確認ください。

[Google Play Services導入時のProguard対応](https://developer.android.com/google/play-services/setup.html#Proguard)


## 疎通テストの実施

マーケットへの申請までに、SDKを導入した状態で十分にテストを行い、アプリケーションの動作に問題がないことを確認してください。

インストール計測の通信は、起動後に一度のみ行わるため、続けて効果測定テストを行いたい場合には、アプリケーションをアンインストールし、再度インストールから行ってください。

* **テスト手順**

1. テスト用端末にテストアプリがインストールされている場合には、アンインストール
1. テスト用端末のデフォルトブラウザのCookieを削除
1. 弊社より発行したテスト用URLをクリック
1. マーケットへリダイレクト
1. テスト用端末にテストアプリをインストール<br />
1. アプリを起動、ブラウザが起動<br />
ここでブラウザが起動しない場合には、正常に設定が行われていません。設定を見直していただき、問題が見当たらない場合には弊社へご連絡ください。
1. LTV地点まで画面遷移<br />
1. アプリを終了し、バックグラウンドからも削除<br />
1. 再度アプリを起動<br />

弊社へ3、6、7、9の時間をお伝えください。正常に計測が行われているか確認いたします。弊社側の確認にて問題がなければテスト完了となります。

> テスト用URLは必ず端末のデフォルトブラウザ上でリクエストされるようにしてください。メールアプリやQRコードアプリを利用されそのアプリ内WebViewで遷移した場合には計測できません。

> テストURLをクリックした際に、遷移先がなくエラーダイアログが表示される場合がありますが、疎通テストにおいては問題ありません。


[リエンゲージメント計測を行う場合のテスト手順](./doc/reengagement_test/ja/)


## その他機能の実装

[プッシュ通知の実装](./doc/notify/ja/)

[オプトアウトの実装](./doc/optout/ja/)

[管理画面上に登録したバンドルバージョンに応じた処理の振り分け](./doc/check_version/ja/)


## 最後に必ずご確認ください（これまで発生したトラブル集）

### URLスキームの設定がされずリリースされたためブラウザからアプリに遷移ができない

Cookie計測を行うために外部ブラウザを起動した後に、元の画面に戻すためにはURLスキームを利用してアプリケーションに遷移させる必要があります。この際、独自のURLスキームが設定されている必要があり、URLスキームを設定せずにリリースした場合にはこのような遷移を行うことができなくなります。

### URLスキームに大文字や記号が含まれ、正常にアプリに遷移されない

環境によって、URLスキームの大文字小文字が判別されないことにより正常にURLスキームの遷移が行えない場合があります。URLスキームは全て小文字の英数字で設定を行ってください。


### URLスキームの設定が他社製アプリと同一でブラウザからそちらのアプリが起動してしまう

iOSにおいて、複数のアプリに同一のURLスキームが設定されていた場合に、どのアプリが起動するかは不定です。確実に特定のアプリを起動することができなくなるため、URLスキームは他社製アプリとはユニークになるようある程度の複雑性のあるものを設定してください。

### 短時間で大量のユーザー獲得を行うプロモーションを実施したら正常に計測がされなかった

iOSには、アプリ起動時に一定時間以上メインスレッドがブロックされるとアプリケーションを強制終了する仕様があります。起動時の初期化処理など、メインスレッド上でサーバーへの同期通信を行わないようにご注意ください。リワード広告などの大量のユーザーを短時間で獲得した結果、サーバーへのアクセスが集中し、通信のレスポンスが非常に悪くなることでアプリケーションの起動に時間がかかり、起動時に強制終了され正常に広告成果が計測できなくなった事例がございます。

以下の手順で、こうした状況をテストすることができますので、以下の設定でアプリケーションが正常に起動するかをご確認ください。

iOS「設定」→「デベロッパー」→「NETWORK LINK CONDITIONER」

* 「Enable」をオン
* 「Very Bad Network」をチェック


### F.O.Xで確認できるインストール数の値がGoogle Play Developer Consoleの数字より大きい

F.O.Xではいくつかの方式を組み合わせて端末の重複インストール検知を行っています。
重複検知が行えない設定では、同一端末でも再インストールされる度にF.O.Xは新規のインストールと判定してしまいます。

重複検知の精度を向上するために、以下の設定を行ってください。

[広告IDを利用するためのGoogle Play Services SDKの導入](./doc/google_play_services/ja/)

[（オプション）外部ストレージを利用した重複排除設定](https://github.com/cyber-z/public_fox_android_sdk/tree/master/doc/external_storage/ja)
