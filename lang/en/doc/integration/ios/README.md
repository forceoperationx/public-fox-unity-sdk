# Setting for iOS project

## **Setting for Xcode project**

### Push value or Xcode project

To create project for iOS, press down Xcode in following  procedure then make necessary setting on Xcode.

1. Select  "File">"BUild Settings..." from menu
2. Select "iOS" on Platform, then push "Switch Platform"
3. Press down "Player Setting", then make appropriate setting for your  environment by Inspector.
4. Press down "Build" or "Build And Run", then publish Xcode

> ※ If `Automatic Reference Counting` is YES, to run through compile, set -fno-objc-arc to compiler flag of following two files.
* LibFoxSdk.m
* FoxVersionPlugin.mm

### Editing Xcode project

Open published Xcode project, then edit it.

* **Setting framework**

Link following framework to project.

<table>
<tr><th>Frameworkname</th><th>Status</th></tr>
<tr><td>SafariServices.framework</td><td>Optional</td></tr>
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>iAd.framework </td><td>Required</td></tr>
<tr><td>Security.framework </td><td>Required </td></tr>
<tr><td>StoreKit.framework </td><td>Required </td></tr>
</table>

> Since AdSupport.framework is the added framework on iO S6 or later, to run the application on iOS 5 or more previous version(set iOS Deployment Target on 5.1 or lesser),  set as "optional" for performing weak link.

> ※ Since SafariServices.framework is the added framework on iOS 9 or later, to run the application on iOS 8 or more previous version(set iOS Deployment Target on 8.4 or lesser),  set as "optional" for performing weak link.

![Setting frame work 01](/lang/en/doc/integration/ios/config_framework/img01.png)

[Detail for setting framework](/lang/en/doc/integration/ios/config_framework/README.md)

* **SDK setting**

Add necessary setting for SDK operation to plist. Create file named AppAdForce.plist on arbitrary place, then input following key and value.

Key | Type | Value
:---: | :---: | :---
APP_ID | String | Force Operation X manager will notice, input that value.
APP_SALT | String | Force Operation X manager will notice, input that value.
APP_OPTIONS | String | Please do not type anything, so please keep it empty.
CONVERSION_MODE | String | 1
ANALYTICS_APP_KEY | String | Force Operation X manager will notice, input that value.<br />No need to set if access analysis is not used.

![Framework setting 01](/lang/en/doc/integration/ios/config_plist/img05.png)

* **About App Transport Security**

If AppTransportSecurity (below, ATS) that is provided from iOS is enabled, make following  setting to make domain as exception of communication destination of F.O.X SDK.

キー | タイプ | 概要
:---: | :---: | :---
NSExceptionDomains|Dictionary|Dictionary that specify exception of ATS
指定ドメイン文字列|Dictionary|create folowing 2 domain <br>・app-adforce.jp<br>・forceoperationx.com
NSExceptionAllowsInsecureHTTPLoads|Boolean|Specify YES to make an exception of ATS
NSIncludesSubdomains|Boolean|Specify YES to aplly  an exceptional setting of ATS to sub domain as well.

![ATS setting ](/lang/en/doc/integration/ios/config_plist/img06.png)

[Detail of SDK setting](/lang/en/doc/integration/ios/config_plist/README.md)

[AppAdForce.plist sample ](/lang/en/doc/integration/ios/config_plist/AppAdForce.plist)

**Note on implementation on iOS 9**

> As performing Cookie measurement, SFSafariViewController uses on iOS9. For F.O.X Unity SDK v2.16 or later, impelemention is
ｒequired to make  controle  by FoxReengagePlugin after starting  SFSafariViewController.

> If it has been removed, please implement FoXReengagePlugin included in unitypakage of the Unity SDK.

### Other

* [Distribution depending on bundle version enrolled on the management screen.](./check_version/README.md)

---
[PREVIOUS](/lang/en/doc/integration/README.md)

[TOP](/lang/en/README.md)
