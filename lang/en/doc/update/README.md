## Updating to latest version

Please refer to the points below when updating the FOX SDK on an app using an older version.

1. Delete all the files of the older version from the app.
2. Add the latest (updated) files to the project.
3. If you added 'Plugins/FoxPlugin' and 'Plugins/FoxAnalyticsSession' to the MainCamera by drag & drop, then after deleting all the older version files, add the two files to the MainCamera again using drag & drop.
4. In case of iOS, after building the project from Unity, recreate AppAdForce.plist file on Xcode launched after the build.

v2.14 and later support Android Advertising ID.
To acquire the Advertising ID, refer to[Importing Google Play Services SDK to use Advertising ID](/lang/en/doc/integration/android/google_play_services/README.md).

Also, re-engagement tracking feature is available in v2.14 and later.
When performing re-engagement tracking on iOS app, add FoxReengagePlugin.m and FoxReengagePlugin.h to the project. In case of Android app, refer to 'Re-engagement tracking implementation'.  

After updating the SDK, make sure you test the set up thoroughly again. Also, if you implement re-engagement tracking, make sure to test that too.

---
[TOP](/lang/en/README.md)
