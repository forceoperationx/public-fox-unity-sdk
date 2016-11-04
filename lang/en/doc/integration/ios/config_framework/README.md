## Detail for setting framework

Select your project in the project navigator.<br>
Select the 「Build Phases」 tab, and Open 「Link Binaries With Libraries」 expander.<br>
Press down add button(+), and select frameworks and libraries.


![Framework setting 01](/lang/en/doc/integration/ios/config_framework/img01.png)

Add following frameworks to your project.

<table>
<tr><th>Framework</th><th>Status</th></tr>
<tr><td>SafariServices.framework</td><td>Optional</td></tr>
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>iAd.framework </td><td>Required</td></tr>
<tr><td>Security.framework </td><td>Required </td></tr>
<tr><td>StoreKit.framework </td><td>Required </td></tr>
</table>

> ※ Set AdSupport.framework as "Optional", if you set under 5.1 at iOS Deployment Target.<br>The reason is that AdSupport.framework needs above iOS6.

> ※ AdSupport.framework is only available on iOS 6 above, so Set AdSupport.framework as "Optional", if you set under 5.1 at iOS Deployment Target.

> ※ SafariServices.framework is only available on iOS 9 above, so set SafariServices.framework as "Optional", if you set under 8.4 at iOS Deployment Target.

![Framework setting 02](/lang/en/doc/integration/ios/config_framework/img02.png)

---
[iOS TOP](/lang/en/doc/integration/ios/README.md)
