## Importing Google Play Services SDK to use Advertising ID

## Compliance to Google Play developer program

Force Operation X Android SDK complies with the Google Play developer program policies. To comply with the developer program policies, the SDK does not acquire the Advertising ID if an immutable device ID (IMEI, MAC address, and Android ID) has been acquired. From August 1 2014, it is necessary to use Advertising ID for the purposes of advertisement based tracking for all new and updated apps. To be compliant to the developer program policies, please follow the following steps.

## Google Play Services SDK

To use Advertising ID, it is necessary to import Google Play Services SDK. If your app does not import Google Play Services SDK already, refer to the following link to import it.

[Setting Up Google Play Services | Android Developers](https://developer.android.com/google/play-services/setup.html)


## Acquiring the Google Play Services SDK

As of December 2014, Google Play Services SDK can be imported into the project as shown below.

Install the Google Play services package from Android SDK Manager if not done already.

* Start Android SDK Manager.
* Check Google Play services under Extras and install it.

![googlePlayServices01](./img01.png)

## Importing Google Play Services SDK into the project

After acquiring Google Play Services package from the Android SDK Manager, copy "${ANDROID_SDK_PATH}/extras/google/google_play_services/libproject/google-play-services_lib" into Plugins/Android folder of the Unity project.


## Configuring Google Play Services for use

#### AndroidManifest.xml

Add the following meta-data to AndroidManifest.xml.

```xml
<meta-data
    android:name="com.google.android.gms.version"
    android:value="@integer/google_play_services_version" />
```

#### Configuring ProGuard

If you obfuscate the source code using ProGuard, add the following rules to ProGuard.

```
-keep class * extends java.util.ListResourceBundle {
    protected Object[][] getContents();
}

-keep public class com.google.android.gms.common.internal.safeparcel.SafeParcelable {
    public static final *** NULL;
}

-keepnames @com.google.android.gms.common.annotation.KeepName class *
-keepclassmembernames class * {
    @com.google.android.gms.common.annotation.KeepName *;
}

-keepnames class * implements android.os.Parcelable {
    public static final ** CREATOR;
}
```

---
[Android TOP](/lang/en/doc/integration/android/README.md)
