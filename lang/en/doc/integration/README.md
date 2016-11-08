## Integrating the Unity plugin

### Adding Unity plugin to the project

1. Launch unity, then select the target project.
2. Select 「Assets」>「Import Package」>「Custom Package」 from menu.
3. Select "FOX-UnityPlugin_.unitypackage".
4. Click "All" button to check all.
5. If the plug in for iOS is unnecessary, remove the checks from Fox/iOS and Plugin/iOS.
6. If the plug in for Android is unnecessary, remove the checks from Fox/Android and Plugin/Android.
7. Click the "Import" button.

<img src="./img01.png" width="400px" />

> ※ Lastest native version of SDK (iOS/Android) is included in `Fox_<version>.unitypackage`

> ※ Please don't import FoxNotifyPlugin.h, and FoxNotifyPlugin.m files if you do not wish to use F.O.X push notifications for iOS. Also, if you do not wish to perform re-engagement measurement, please do not import FoxReengagePlugin.h, and FoxReengagePlugin.m files.

#### **Note about iOS 9 implementation**

> In case of cookie based measurement on iOS 9 onwards, F.O.X makes use of SFSafariViewController. In F.O.X Unity SDK v2.16 onwards, it is necessary to import FoxReengagePlugin as SFSafariViewController is handled in FoxReengagePlugin.  

> If you have not included FoxReengagePlugin in your project, please use the unitypackage file of F.O.X Unity SDK to import FoxReengagePlugin.


---
[TOP](/lang/en/README.md)
