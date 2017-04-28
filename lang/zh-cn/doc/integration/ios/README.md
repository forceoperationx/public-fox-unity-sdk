# iOS項目的設定

## **Xcode項目的設定**

### Xcode項目的發行

為了做成iOS用的項目，按下面的步驟發行Xcode項目，在Xcode上進行必要的設定。

1. 選擇「File」>「Build Settings…」
2. 選擇Platform的「iOS」、按下「Switch Platform」
3. 按下「Player Settings」、使用Inspector結合自己的環境進行設定
4. 	按下「Build」或者「Build And Run」、從Xcode項目進行發行。

> ※ 把`Automatic Reference Counting`設定為YES的時候、為了通過編譯，請在Xcode Build Phases裡將下面兩個文件的`compiler flags`設定為`-fno-objc-arc`。
* LibFoxSdk.m
* FoxVersionPlugin.mm

### Xcode項目的編輯

打開並編輯發行的Xcode項目。

* **Framework設定**

請把下面的Framework追加到開發項目裡。

<table>
<tr><th>Framework名</th><th>Status</th></tr>
<tr><td>SafariServices.framework</td><td>Optional</td></tr>
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>Security.framework </td><td>Required </td></tr>
<tr><td>StoreKit.framework </td><td>Required </td></tr>
</table>

> ※SafariServices.framework是在iOS 9之後被追加的Framework，如果要讓APP在iOS 8之前版本也能運行（將iOS Deployment Target設定為8.4以下），為了設定為weak link，請修改設置為”Optional”

![Framework設定01](/lang/zh-tw/doc/integration/ios/config_framework/img01.png)

[Framework設定的詳細](/lang/zh-tw/doc/integration/ios/config_framework/README.md)

* **SDK設定**

追加必要的設定到plist裡讓SDK發揮作用。
請在FOX管理畫面裡（SDK導入→平台的選擇→SDK導入文檔→SDK導入步驟→設定文件的下載）下載設定文件「AppAdForce.plist」，並根據自己項目的需要進行修改。<br />
也可以按下方所示，新建「AppAdForce.plist」這樣一個Property List文件放到項目的任意一個地方，並手動輸入下面的Key和Value。

Key | Type | Value
:---: | :---: | :---
APP_ID | String | 請將由Force Operation X管理者通知的數值輸入。
SERVER_URL | String | 請將由Force Operation X管理者通知的數值輸入。
APP_SALT | String | 請將由Force Operation X管理者通知的數值輸入。
APP_OPTIONS | String | 請空白。
CONVERSION_MODE | String | 請輸入1
ANALYTICS_APP_KEY | String | 請將由Force Operation X管理者通知的數值輸入。<br />不利用流量分析的場合沒有必要設定
ANALYTICS_SERVER_URL | String | 請將由Force Operation X管理者通知的數值輸入。<br />不利用流量分析的場合沒有必要設定

![Framework設定01](/lang/zh-tw/doc/integration/ios/config_plist/img05.png)

[SDK設定的詳細](/lang/zh-tw/doc/integration/ios/config_plist/README.md)

[AppAdForce.plist例子](/lang/zh-tw/doc/integration/ios/config_plist/AppAdForce.plist)

**在iOS9環境導入的注意點**

> 如果到目前為止都沒有安裝，請導入同捆的FoxReengagePlugin到這個Unity SDK的unitypackage文件裡。

---
[TOP](/lang/zh-tw/README.md)
