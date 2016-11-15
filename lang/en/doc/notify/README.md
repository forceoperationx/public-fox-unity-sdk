## Implementing push notifications

Using the FOX push notifications feature, it is possible to send out notifications to a group of users brought in by a particular ad campaign. Implementation of this feature can be skipped if you do not plan to use FOX push notifications.

## Common configurations for iOS/Android

### Registering the device

FOX uses APN (Apple Push Notification Service) for iOS, and GCM（Google Cloud Messaging for Android）for Android to send push notifications.

To register the device in APN or GCM please follow the steps below.

Add the following code to the script that runs on app launch.

> If the script uses sendConversion() method, then make sure to add the following code after the sendConversion() call.

```cs
#if UNITY_IOS || UNITY_IPHONE		FoxPlugin.registerForRemoteNotifications();#elif UNITY_ANDROID
		// Input the Google Developer Console Project ID in place of xxxxxx
		FoxPlugin.registerForRemoteNotifications(××××××);#endif
```

## iOS specific configuration

Usually, the above common configuration is enough to setup push notifications.
On app's first run, we acquire the device token from Apple servers and send it to FOX servers.
FOX sends push notifications using the device token.
Please follow the steps below if your app requires push notifications other than the ones sent by FOX, too.
#### When using push notifications other than the ones sent by FOX, on iOS

It is required to register the device token on FOX's servers.

1. Delete FoxNotifyPlugin.h and FoxNotifyPlugin.m included in the FOX SDK.
2. Open Classes/AppController.mm file for Xcode that is created after building the Unity project.
3. Send the device token to FOX servers upon receiving the same from Apple.

If the device token is acquired successfully, the following code is necessary to send it to FOX servers as `didRegisterForRemoteNotificationsWithDeviceToken:` application delegate gets called.

```objective-c
#import "Notify.h"

- (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken{
		// method to register the device on FOX servers    [[Notify sharedManager] manageDevToken:deviceToken];    UnitySendDeviceToken(deviceToken);}
```

4. Add the method to send a response to FOX upon receiving a push notification

Implement the `application:didFinishLaunchingWithOptions:` and `application:didReceiveRemoteNotification:` methods as shown below.

```objective-c
- (BOOL)application:(UIApplication *)application
   didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
	// ...

#if !TARGET_IPHONE_SIMULATOR
	[[Notify sharedManager] sendOpenedStatus: launchOptions];
#endif

	// ...
}
```

```objective-c
- (void)application:(UIApplication *)application
	 didReceiveRemoteNotification:(NSDictionary *)userInfo {

#if !TARGET_IPHONE_SIMULATOR
	if ( [[Notify sharedManager] sendOpenedStatus:userInfo application:application] ) {
		return;
	}
#endif

}
```

## Android specific configuration

### Setting up the permissions

Add the necessary permissions for receiving push notifications inside the \<manifest\> tag.

```xml
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="applicationPackageName.permission.C2D_MESSAGE" />
<permission android:name="applicationPackageName.permission.C2D_MESSAGE" android:protectionLevel="signature" />
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
```

### Setting up the receiver for push notifications

Add the following receiver in the \<application\> tag that will handle the push notifications.

```xml
<receiver android:name="jp.appAdForce.android.NotifyReceiver"
	android:permission="com.google.android.c2dm.permission.SEND">
	<intent-filter>
		<action android:name="com.google.android.c2dm.intent.RECEIVE" />
		<action android:name="com.google.android.c2dm.intent.REGISTRATION" />
		<category android:name="applicationPackageName" />
	</intent-filter>
</receiver>
```

### When using two receivers

Only one receiver class can be registered for com.google.android.c2dm.intent.RECEIVE and com.google.android.c2dm.intent.REGISTRATION. When using more than one receivers, perform the steps below.

```xml
<meta-data
	android:name="APPADFORCE_NOTIFY_RECEIVER"
	android:value="Receiver class other than FOX receiver" />
```

jp.appAdForce.android.NotifyReceiver calls onResume(), or onMessage(), or onRegistered() methods of the specified receiver class.

---
[TOP](/lang/en/README.md)
