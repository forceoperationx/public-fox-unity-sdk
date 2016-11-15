## More on framework configuration

Click on Build Target, and select 'Build Phased' → 'Link Binary with Libraries'. Select each framework by clicking the '+' button.

![Framework setting 01](/lang/en/doc/integration/ios/config_framework/img01.png)

Link the following frameworks to the project.

<table>
<tr><th>Framework</th><th>Status</th></tr>
<tr><td>SafariServices.framework</td><td>Optional</td></tr>
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>iAd.framework </td><td>Required</td></tr>
<tr><td>Security.framework </td><td>Required </td></tr>
<tr><td>StoreKit.framework </td><td>Required </td></tr>
</table>

> Since AdSupport.framework was added in iOS 6, to run the app on devices running iOS 5 and older versions (set iOS Deployment Target to 5.1 or below), set status to "Optional" to perform weak linking.

> ※ Since SafariServices.framework was added in iOS 9, to run the app on devices running iOS 8 and older versions (set iOS Deployment Target to 8.4 or below), set status to "Optional" to perform weak linking.

![Framework setting 02](/lang/en/doc/integration/ios/config_framework/img02.png)

---
[iOS TOP](/lang/en/doc/integration/ios/README.md)
