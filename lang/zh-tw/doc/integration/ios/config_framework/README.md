## Framework設定的詳細

點擊編譯對象，「Build Phases」→選擇「Link Binary With Libraries」。點擊「＋」按鈕，請選擇各Framework。

![Framework設定01](/lang/zh-tw/doc/integration/ios/config_framework/img01.png)

請把下面的Framework追加到開發項目裡。

<table>
<tr><th>Framework名</th><th>Status</th></tr>
<tr><td>SafariServices.framework</td><td>Optional</td></tr>
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>iAd.framework </td><td>Required</td></tr>
<tr><td>Security.framework </td><td>Required </td></tr>
<tr><td>StoreKit.framework </td><td>Required </td></tr>
</table>

> ※SafariServices.framework是在iOS 9之後被追加的Framework，如果要讓APP在iOS 8之前版本也能運行（將iOS Deployment Target設定為8.4以下），為了設定為weak link，請修改設置為”Optional”

![Framework設定02](/lang/zh-tw/doc/integration/ios/config_framework/img02.png)

---
[iOS TOP](/lang/zh-tw/doc/integration/ios/README.md)
