# iOSプロジェクトの設定

## **Xcodeプロジェクトの設定**

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
<tr><td>SafariServices.framework</td><td>Optional</td></tr>
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>iAd.framework </td><td>Required</td></tr>
<tr><td>Security.framework </td><td>Required </td></tr>
<tr><td>StoreKit.framework </td><td>Required </td></tr>
</table>

> AdSupport.frameworkはiOS 6以降で追加されたフレームワークのため、アプリケーションをiOS 5以前でも動作させる(iOS Deployment Targetを5.1以下に設定する)場合にはweak linkを行うために”Optional”に設定してください。

> ※SafariServices.frameworkはiOS 9以降で追加されたフレームワークのため、アプリケーションをiOS 8以前でも動作させる(iOS Deployment Targetを8.4以下に設定する)場合にはweak linkを行うために”Optional”に設定してください。

![フレームワーク設定01](/lang/ja/doc/integration/ios/config_framework/img01.png)

[フレームワーク設定の詳細](/lang/ja/doc/integration/ios/config_framework/)

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

![フレームワーク設定01](/lang/ja/doc/integration/ios/config_plist/img05.png)

* **App Transport Securityについて**

iOS9より提供されたAppTransportSecurity(以下、ATS)を有効にしている場合、Info.plistに以下の設定を行いF.O.X SDKが行う通信先のドメインをATSの例外としてください。

キー | タイプ | 概要
:---: | :---: | :---
NSExceptionDomains|Dictionary|ATSの例外を指定するディクショナリー
指定ドメイン文字列|Dictionary|以下２つのドメインをキーで作成してください。<br>・app-adforce.jp<br>・forceoperationx.com
NSExceptionAllowsInsecureHTTPLoads|Boolean|YES を指定してくださいATSの例外とします。
NSIncludesSubdomains|Boolean|YES を指定しATSの例外設定をサブドメインにも適用させます。

![ATS設定](/lang/ja/doc/integration/ios/config_plist/img06.png)

[SDK設定の詳細](/lang/ja/doc/integration/ios/config_plist/README.md)

[AppAdForce.plistサンプル](/lang/ja/doc/integration/ios/config_plist/AppAdForce.plist)

**iOS9における導入の注意点**

> Cookie計測を実施する際に、iOS9ではSFSafariViewControllerが使用します。
F.O.X Unity SDK v2.16以降では、SFSafariViewController起動後の制御をFoxReengagePluginで行うため導入が必須となります。

> これまで外されていた場合には、本Unity SDKのunitypackageファイルに同梱のFoxReengagePluginをご導入ください。

---
[TOPへ](/lang/ja/)
