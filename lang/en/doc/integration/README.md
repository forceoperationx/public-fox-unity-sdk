## Implementation of Unity plugin

### Adding Unity plugin to project

1. Activate unity, then select Unity project to incorporate plugin.
2. Select 「Assets」>「Import Package」>「Custom Package」 from menu.
3. Select "FOX-UnityPlugin_.unitypackage"
4. Select "All" button to check all
5. If the plug in for iOS is unnecessary, remove the checks from Fox/iOS and Plugin/iOS
6. If the plugin for Android is unnecessary, remove the checks from Fox/Android and Plugin/Android
7. Select "Import" button

<img src="./img01.png" width="400px" />

> ※ Lastest native version of SDK (iOS/Android) is included in `Fox_<version>.unitypackage`

> ※ If you do not use push notice of F.O.X in iOS, do not import FoxNotifyPlugin.h, FoxNotifyPlugin.m. Also, IF you do not perform reengagement measurement, do not import FoxReengagePlugin.h, FoxReengagePlugin.m.

#### **Note about implementing iOS**

> As performing Cookie measurement, SFSafariViewController is used on iOS. For v2.16 or later, order to make the control of SFSafariViewController after startup, implementing FoxReengagePlugin is necessary.

> If you have not set, please use FoxReengagePlugin included in unitypackage of SDK.


---
[TOP](/lang/en/README.md)
