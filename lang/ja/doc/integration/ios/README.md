[TOP](../../../README.md)　>　[Unityプラグインの導入手順](../README.md)　>　**iOS プロジェクトの詳細設定**

---

# iOS プロジェクトの詳細設定

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
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>Security.framework</td><td>Required </td></tr>
<tr><td>SystemConfiguration.framework</td><td>Required </td></tr>
<tr><td>WebKit.framework</td><td>Required </td></tr>
<tr><td>WebKit.framework</td><td>Required </td></tr>
<tr><td>CoreTelephony.framework</td><td>Optional</td></tr>
</table>

* **App Transport Securityについて**

F.O.X SDK ver4.0.0からは計測における全ての通信をHTTPSを利用して行うため、追加で対応を行う必要はありません。


---
[戻る](../README.md)

[トップ](../../../README.md)
