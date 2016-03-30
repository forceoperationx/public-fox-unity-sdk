# 広告配信機能

## 1. 環境設定

### 1.1 Android

組み込み対象のアプリにはGooglePlayServicesをご導入の上、AdvertisingIDを取得出来ることが必須となっております。<br>
AdvertisingIDを取得するには[`こちら`](/lang/ja/doc/google_play_services/README.md)をご確認ください。

### AndroidManifest.xmlの設定

**[Activityの追加]**

インタースティシャル広告を表示する際に必須となるActivityとなります。<br>
以下、そのままコピーして&lt;application&gt;タグ内にご設定ください。

```xml
<activity
    android:name="co.cyberz.dahlia.DahliaActivity"
    android:theme="@android:style/Theme.Translucent" />
```

### 1.2 iOS

### 必須framework

* UIKit.framework
* Foundation.framework
* AdSupport.framework
* SystemConfiguration.framework
* Security.framework

### 必須ライブラリ

|ファイル名|修正|
|:---:|:---|
|DLBannerView.h|バナー広告表示用ライブラリヘッダ|
|DLInterstitialViewController.h|インタースティシャル広告表示用ライブラリヘッダ|
|DLAdStateDelegate.h|広告表示イベント処理のデリゲート|
|DLUAdStateDelegateImp.h|広告表示イベント処理用デリゲートのラッパー|
|DLUAdStateDelegateImp.m|広告表示イベント処理用デリゲートのラッパー|
|DLUInterface.h|広告表示用ライブラリのラッパー|
|DLUInterface.m|広告表示用ライブラリのラッパー|

## 2. API

### DahliaBannerAds

|返り値型|メソッド|詳細|
|---:|:---|:---|
|void|load ( String placementId, AdPosition position )<br><br>`placementID` : 広告表示ID (管理者より発行されます)<br>`position` : 配置位置|バナー広告を表示します。|
|void|hide (  )|バナー広告を非表示にします。|

|イベントハンドラ|詳細|
|:---|:---|
|AdSuccess|正常に広告が表示された場合に呼ばれます。|
|AdFailed|広告の表示に失敗した場合に呼ばれます。|


### AdPosition

|パラメータ|詳細|
|:---:|:---|
|TOP|画面上部の中心に配置します。|
|BOTTOM|画面下部の中心に配置します。|


### DahliaInterstitialAds

|返り値型|メソッド|詳細|
|---:|:---|:---|
|void|show ( String placementId )<br><br>`placementID` : 広告表示ID (管理者より発行されます)|インタースティシャル広告を表示します。|

|イベントハンドラ|詳細|
|:---|:---|
|AdSuccess|正常に広告が表示された場合に呼ばれます。|
|AdFailed|広告の表示に失敗した場合に呼ばれます。|
|AdClosed|広告が閉じられた場合に呼ばれます。|

> インタースティシャル広告が表示できなかった場合、`AdFailed`が呼ばれた後にも`AdClosed`は呼ばれます。

## 3. コードへの組み込み

### 広告表示サンプル

```cs
using System;
using UnityEngine;
using DahliaAds.Api;

public class ViewAdsDemoScript : MonoBehaviour
{

    private DahliaBannerAds dba;
	  private DahliaInterstitialAds dInterstitial;

    void Start() {
  		dba = new DahliaBannerAds();
  		dba.AdSuccess += HandleDahliaBannerAdSuccess;
  		dba.AdFailed += HandleDahliaBannerAdFailed;
  		dInterstitial = new DahliaInterstitialAds();
  		dInterstitial.AdClosed += HandleDahliaInterstitialClosed;
  		dInterstitial.AdSuccess += HandleDahliaInterstitialAdSuccess;
  		dInterstitial.AdFailed += HandleDahliaInterstitialAdFailed;
  	}

    void OnGUI()
    {
        GUI.skin.button.fontSize = (int) (0.05f * Screen.height);
        GUI.skin.label.fontSize = (int) (0.025f * Screen.height);

        Rect requestBannerRect = new Rect(0.1f * Screen.width, 0.05f * Screen.height,
                                   0.8f * Screen.width, 0.1f * Screen.height);
		    if (GUI.Button(requestBannerRect, "Banner Top"))
        {
			       dba.load ("バナー広告表示ID", DahliaAds.Api.AdPosition.TOP);
        }

        Rect showBannerRect = new Rect(0.1f * Screen.width, 0.175f * Screen.height,
                                       0.8f * Screen.width, 0.1f * Screen.height);
		    if (GUI.Button(showBannerRect, "Banner Bottom"))
        {
			       dba.load ("バナー広告表示ID", DahliaAds.Api.AdPosition.BOTTOM);
		    }

        Rect hideBannerRect = new Rect(0.1f * Screen.width, 0.3f * Screen.height,
                                       0.8f * Screen.width, 0.1f * Screen.height);
        if (GUI.Button(hideBannerRect, "Hide Banner"))
        {
			       dba.hide();
        }

        Rect showInterstitialRect = new Rect(0.1f * Screen.width, 0.675f * Screen.height,
                                             0.8f * Screen.width, 0.1f * Screen.height);
        if (GUI.Button(showInterstitialRect, "Show Interstitial"))
        {
			       dInterstitial.show("インタースティシャル広告表示ID");
        }
    }

    // --- バナー イベントハンドラ ---

	  public void HandleDahliaBannerAdSuccess(object sender, EventArgs args) {
		    print ("HandleDahliaBannerAdSuccess event received.");
	  }

	  public void HandleDahliaBannerAdFailed(object sender, EventArgs args) {
  		print ("HandleDahliaBannerAdFailed event received.");
	  }

    // --- インタースティシャル イベントハンドラ ---

	  public void HandleDahliaInterstitialClosed(object sender, EventArgs args) {
  		print("HandleDahliaInterstitialClosed event received.");
	  }

	  public void HandleDahliaInterstitialAdSuccess(object sender, EventArgs args) {
		  print ("HandleDahliaInterstitialSuccess event received.");
	  }

	  public void HandleDahliaInterstitialAdFailed(object sender, EventArgs args) {
		  print ("HandleDahliaInterstitialFailed event received.");
	  }
}
```

## 4. 表示サンプル

### バナー広告サンプル

<table>
<tr>
<td align="center" style="border-style:none;">[バナー広告サンプル]</td>
<td align="center" style="border-style:none;">[インタースティシャル広告サンプル]</td>
</tr>
<tr>
<td style="border-style:none;"><img src="./sample_banner.png" width="300px"/></td>
<td style="border-style:none;"><img src="./sample_interstitial.png" width="300px"/></td>
</tr>
</table>

---
[トップ](/lang/ja/README.md)
