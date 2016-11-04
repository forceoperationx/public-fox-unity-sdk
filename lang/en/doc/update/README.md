## About update to latest version

By following procedures below, it is able to update to latest version of SDK.
In this section instructs the update procedures of latest version of SDK.

1. Delete the old version of SDK in your project and add the latest version of SDK to your project
2. In case of implementation of installation measurement by Inspector, delete old version of SDK files in MainCamera, and drag & drop new version of SDK files to MainCamera.
3. 「Plugins/FoxPlugin」及び「Plugins/FoxAnalyticsSession」をMainCamera等にドラッグ＆ドロップに追加することで実装を行っていた場合は、古いバージョンのファイルを全て消した上で、再度新しいファイルをドラッグ＆ドロップで追加してください。
4. In case of iOS application, please re-create AppAdForce.plist in Xcode.


In case of Android application, F.O.X SDK has the function to obtain the Google Advertising ID since its version 2.14.
If you need to obtain the Google Advertinsing ID, please refer to [The implementation of Google Play Services SDK to use advertisement ID](/lang/en/doc/integration/android/google_play_services/README.md).

After update to latest version of SDK, perform your test and make sure that there is no problem in the operation of the application.


---
[TOP](/lang/en/README.md)
