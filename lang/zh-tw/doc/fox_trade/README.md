# 廣告投放功能

## 1. 環境設定

### 1.1 Android

需要在嵌入對象的APP裡導入GooglePlayServices來取得AdvertisingID。
關於取得AdvertisingID請參考[`這裏`](/lang/zh-tw/doc/google_play_services/README.md)

### AndroidManifest.xml的設定

**[Activity的追加]**

表示插播廣告時必須使用Activity。<br>
請按下面那樣原封不動地拷貝到&lt;application&gt;標籤裡。

```xml
<activity
    android:name="co.cyberz.dahlia.DahliaActivity"
    android:theme="@android:style/Theme.Translucent" />
```

### 1.2 iOS

### 必要的framework

* UIKit.framework
* Foundation.framework
* AdSupport.framework
* SystemConfiguration.framework
* Security.framework

### 必要的類庫

|文件名|修正|
|:---:|:---|
|DLBannerView.h|橫幅廣告表示用類庫頭文件|
|DLInterstitialViewController.h|插播廣告表示用類庫頭文件|
|DLAdStateDelegate.h|廣告表示事件處理的Delegate|
|DLUAdStateDelegateImp.h|廣告表示事件處理使用Delegate封裝的頭文件|
|DLUAdStateDelegateImp.m|廣告表示事件處理使用Delegate封裝|
|DLUInterface.h|廣告表示用類庫封裝的頭文件|
|DLUInterface.m|廣告表示用類庫封裝|

## 2. API

### DahliaBannerAds

|返回值類型|方法|詳細|
|---:|:---|:---|
|void|load ( String placementId, AdPosition position )<br><br>`placementID` : 廣告表示ID (管理員發行)<br>`position` : 配置位置|表示橫幅廣告。|
|void|hide (  )|不表示橫幅廣告。|

|EventHandler|詳細|
|:---|:---|
|AdSuccess|廣告正常表示時被調用。|
|AdFailed|廣告表示失敗時被調用。|


### AdPosition

|參數|詳細|
|:---:|:---|
|TOP|配置到畫面上部的中心。|
|BOTTOM|配置到畫面下部的中心。|
|TOP_LEFT|配置到畫面上部的左側。|
|TOP_RIGHT|配置到畫面上部的右側。|
|BOTTOM_LEFT|配置到畫面下部的左側。|
|BOTTOM_RIGHT|配置到畫面下部的右側。|


### DahliaInterstitialAds

|返回值|方法|詳細|
|---:|:---|:---|
|void|show ( String placementId )<br><br>`placementID` : 廣告表示ID (管理員發行)|表示插播廣告。|

|EventHandler|詳細|
|:---|:---|
|AdSuccess|廣告正常表示時被調用。|
|AdFailed|廣告表示失敗時被調用。|
|AdClosed|廣告被關閉時被調用。|

> 不能表示插播廣告的時候，`AdFailed`被調用後，`AdClosed`也會被調用。

## 3. 嵌入到代碼

### 廣告表示範例

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
			       dba.load ("橫幅廣告表示ID", DahliaAds.Api.AdPosition.TOP);
        }

        Rect showBannerRect = new Rect(0.1f * Screen.width, 0.175f * Screen.height,
                                       0.8f * Screen.width, 0.1f * Screen.height);
		    if (GUI.Button(showBannerRect, "Banner Bottom"))
        {
			       dba.load ("橫幅廣告表示ID", DahliaAds.Api.AdPosition.BOTTOM_LEFT);
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
			       dInterstitial.show("插播廣告表示ID");
        }
    }

    // --- 橫幅廣告EventHandler ---

	  public void HandleDahliaBannerAdSuccess(object sender, EventArgs args) {
		    print ("HandleDahliaBannerAdSuccess event received.");
	  }

	  public void HandleDahliaBannerAdFailed(object sender, EventArgs args) {
  		print ("HandleDahliaBannerAdFailed event received.");
	  }

    // --- 插播廣告EventHandler---

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

## 4. 表示範例

### 橫幅廣告範例

<table>
<tr>
<td align="center" style="border-style:none;">[橫幅廣告範例]</td>
<td align="center" style="border-style:none;">[插播廣告範例]</td>
</tr>
<tr>
<td style="border-style:none;"><img src="./sample_banner.png" width="300px"/></td>
<td style="border-style:none;"><img src="./sample_interstitial.png" width="300px"/></td>
</tr>
</table>

---
[Top](/lang/zh-tw/README.md)
