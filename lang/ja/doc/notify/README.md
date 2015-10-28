## プッシュ通知の実装

F.O.Xのプッシュ通知では、広告流入元別にユーザーのセグメントを分けてプッシュ通知を配信することが可能です。F.O.Xのプッシュ通知機能を使用しない場合は、本設定の必要はありません。


## iOS/Androidの共通設定

### 端末の登録

F.O.Xのプッシュ通知は、iOSの場合はAPNs（Apple Push Notification Service）を、Androidの場合はGoogleが提供するGCM（Google Cloud Messaging for Android）を利用してプッシュ通知を行います。

APNs及びGCMに端末を登録するために下記の設定を行います。

起動時に実行されるスクリプトに、次の処理を追加します。

> sendConversionメソッドをスクリプト実装している場合は、必ずsendConversionメソッドが呼ばれる後に配置してください。

```C#
#if UNITY_IOS || UNITY_IPHONE		FoxPlugin.registerForRemoteNotifications();#elif UNITY_ANDROID
		// ××××××にはGoogle Developers Consoleで取得したProject番号を入力してください。		FoxPlugin.registerForRemoteNotifications(××××××);#endif
```

## iOS用の設定

通常は前述の共通設定のみで設定終了です。初回起動時に、Appleのサーバからデバイストークンを取得し、F.O.Xのサーバへ送信します。そのデバイストークンを使用して、F.O.X からプッシュ通知を送信します。
もし、F.O.X以外のプッシュ通知機能を共存させる場合は次の実装を行ってください。

#### iOSでF.O.X以外のプッシュ通知機能を共存させる場合

デバイストークンをF.O.Xサーバーに登録する必要があります。

1. F.O.X SDKに含まれるFoxNotifyPlugin.hとFoxNotifyPlugin.mを削除します。
2. Unityプロジェクトをビルドした後に立ち上がるXcode上のClasses/AppController.mmを開きます
3. Appleからデバイストークンを受信した際にF.O.Xへとトークンを送信するようにします

デバイストークンの取得に成功した場合、Application DelegateのdidRegisterForRemoteNotificationsWithDeviceToken:が呼び出されますので、 取得したデバイストークンをF.O.Xへ送信するために、次の通り実装を行ってください。

```objectivec
#import "Notify.h"

- (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken{
    // F.O.Xに端末を登録するメソッド    [[Notify sharedManager] manageDevToken:deviceToken];
    UnitySendDeviceToken(deviceToken);}
```

4. プッシュ通知を受信した際に、F.O.Xへ開封通知を送信するメソッドを追加

application:didFinishLaunchingWithOptions:とapplication:didReceiveRemoteNotificationに下記の実装を行ってください。

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

## Android用の設定

### パーミッションの設定

下記のように、プッシュ通知を受け取るために必要なパーミッションの設定を\<manifest\>タグ内に追加してください。

```xml
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="アプリのパッケージ名.permission.C2D_MESSAGE" />
<permission android:name="アプリのパッケージ名.permission.C2D_MESSAGE" android:protectionLevel="signature" />
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
```

### プッシュ通知用レシーバーの設定

下記のように、プッシュ通知を受け取るために必要なレシーバーの設定を\<application\>タグ内に追加してください。

```xml
<receiver android:name="jp.appAdForce.android.NotifyReceiver"
	android:permission="com.google.android.c2dm.permission.SEND">
	<intent-filter>
		<action android:name="com.google.android.c2dm.intent.RECEIVE" />
		<action android:name="com.google.android.c2dm.intent.REGISTRATION" />
		<category android:name="アプリのパッケージ名" />
	</intent-filter>
</receiver>
```

### 二つのレシーバーを共存させる場合

com.google.android.c2dm.intent.RECEIVEとcom.google.android.c2dm.intent.REGISTRATIONに対するレシーバークラスは一つしか選択できません。アプリケーションが二つのレシーバークラスを必要とする場合は、以下の設定を追記してください。

```xml
<meta-data android:name="APPADFORCE_NOTIFY_RECEIVER" android:value="共存させたいF.O.X以外のレシーバークラス" />
```

内部的にはjp.appAdForce.android.NotifyReceiverクラスから、共存させたいレシーバークラスのonResume()、もしくはonMessage()、onRegistered()を呼び出します。

---
[TOPへ](/lang/ja/)
