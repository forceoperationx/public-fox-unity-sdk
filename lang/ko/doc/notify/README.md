## 푸시 알림 구현

F.O.X 푸시 알림은 광고 유입원 별로 사용​​자 세그먼트를 나누어 푸시 알림을 제공 할 수 있습니다. F.O.X 푸시 알림 기능을 사용하지 않는 경우는 본 설정이 필요 없습니다.


## iOS / Android 공통 설정

### 단말기 등록

F.O.X 푸시 알림은 iOS의 경우는 APNs(Apple Push Notification Service)를 Android의 경우는 Google이 제공하는 GCM(Google Cloud Messaging for Android)를 이용하여 푸시 알림을 합니다.

APNs 및 GCM에 단말기를 등록하기 위해서는 아래의 설정을합니다.

기동시 실행되는 스크립트에 다음의 처리를 추가합니다.

> sendConversion 메소드를 스크립트 구현하고 있는 경우는 반드시 sendConversion 메소드를 호출후에 배치 하십시오.

```C#
#if UNITY_IOS || UNITY_IPHONE
	FoxPlugin.registerForRemoteNotifications();
#elif UNITY_ANDROID
	// ××××××에는 Google Developers Console에서 취득한 Project 번호를 입력하십시오.
	FoxPlugin.registerForRemoteNotifications(××××××);
#endif
```

## iOS용 설정

일반적으로 위의 공통 설정만으로 설정 완료입니다.
처음 기동할 때, Apple서버에서 디바이스 토큰을 취득해, F.O.X 서버에 보냅니다.
그 디바이스 토큰을 사용하여 F.O.X에서 푸시 알림을 보낼 수 있습니다.
만약 F.O.X 이외의 푸시 알림 기능을 함께 사용하는 경우 다음의 구현을 해야합니다.

#### iOS에서 FO​​X 이외의 푸시 알림 기능을 함께 사용하는 경우

디바이스 토큰을 F.O.X 서버에 등록할 필요가 있습니다.

1. F.O.X SDK에 포함된 FoxNotifyPlugin.h와 FoxNotifyPlugin.m을 삭제합니다.
2. Unity 프로젝트를 빌드한 후 Xcode에서 Classes/AppController.mm을 엽니다
3. Apple에서 디바이스 토큰을 수신했을때 F.O.X로 토큰을 보내도록 합니다

디바이스 토큰의 취득에 성공했을 경우, Application Delegate의 didRegisterForRemoteNotificationsWithDeviceToken: 이 호출되므로 취득한 디바이스 토큰을 F.O.X에 전송하기 위해 다음과 같이 구현을해야합니다.

```objectivec
#import "Notify.h" 

- (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken{
    // F.O.X에 단말기를 등록하는 메소드
    [[Notify sharedManager] manageDevToken:deviceToken];
    
    UnitySendDeviceToken(deviceToken);
}
```

4. 푸시 알림을 수신했을때, F.O.X에 개봉 알림을 보내는 메소드를 추가

application:didFinishLaunchingWithOptions:와application:didReceiveRemoteNotification 에 아래의 구현을 해야합니다.

```objectivec
- (BOOL)application:(UIApplication *)application
   didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
	// ...

#if !TARGET_IPHONE_SIMULATOR
	[[Notify sharedManager] sendOpenedStatus: launchOptions];
#endif

	// ...
}
```

```objectivec
- (void)application:(UIApplication *)application
	 didReceiveRemoteNotification:(NSDictionary *)userInfo {

#if !TARGET_IPHONE_SIMULATOR
	if ( [[Notify sharedManager] sendOpenedStatus:userInfo application:application] ) {
		return;
	}
#endif

}
```

## Android용 설정

### 퍼미션 설정

아래와 같이 푸시 알림을 받을데 필요한 퍼미션 설정을\<manifest\>태그에 추가하십시오.

```xml
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="앱 패키지명.permission.C2D_MESSAGE" />
<permission android:name="앱 패키지명.permission.C2D_MESSAGE" android:protectionLevel="signature" />
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
```

### 푸시 알림 용 수신기 설정

아래와 같이 푸시 알림을받을 데 필요한 수신기의 설정을 \<application \> 태그에 추가하십시오.

```xml
<receiver android:name="jp.appAdForce.android.NotifyReceiver"
	android:permission="com.google.android.c2dm.permission.SEND">
	<intent-filter>
		<action android:name="com.google.android.c2dm.intent.RECEIVE" />
		<action android:name="com.google.android.c2dm.intent.REGISTRATION" />
		<category android:name="앱 패키지 명" />
	</intent-filter>
</receiver>
```

### 두 수신기를 공존시키는 경우

com.google.android.c2dm.intent.RECEIVE과 com.google.android.c2dm.intent.REGISTRATION 대한 receiver클래스는 하나 밖에 선택할 수 없습니다. 앱이 두개의 receiver클래스를 필요로 하는 경우에는 아래 설정을 추가 기입 해주십시오.

```xml
<meta-data android:name="APPADFORCE_NOTIFY_RECEIVER" android:value="공존시키고 싶은 F.O.X 이외의 receiver클래스"/>
```

내부적으로는 jp.appAdForce.android.NotifyReceiver 클래스에서 공존시키고 싶은 receiver클래스의 onResume() 또는 onMessage(), onRegistered()를 호출합니다.