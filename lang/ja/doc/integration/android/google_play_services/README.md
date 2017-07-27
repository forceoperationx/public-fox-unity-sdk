# 広告IDを利用するためのGoogle Play Services SDKの導入

## Google Playデベロッパープログラムへの準拠

Force Operation X Android SDKはGoogle Playデベロッパープログラムポリシーに準拠しています。本SDKはポリシーに準拠するために、永続的なデバイス ID（IMEI、MACアドレス及びAndroidID）が取得される場合には広告IDが取得されません。2014年8月1日から、Google Playストアにアップロードされたすべての更新や新着アプリには、広告目的で使用する端末IDには広告ID を利用する必要があります。本ポリシーに準拠するために、本ページの手順に従いGooglePlayServicesをご導入ください。

## Google Play Servicesの導入

下記のように、build.gradleに最新のGoogle Play servicesへのdependenciesの設定を追記します。

```
dependencies {
	compile 'com.google.android.gms:play-services:9.4.0'
}
```

> ※ Google Play Servicesの最新バージョンは[`Setting Up Google Play Services | Android Developers`](https://developer.android.com/google/play-services/setup.html)をご確認ください。


> ※ ADTのEclipseサポート終了に伴いEclipseでの導入は廃止しました。<br>開発されている環境がEclipseの場合、[Google Play Services Jar Resolver Library for Unity](https://github.com/googlesamples/unity-jar-resolver)を用いてGooglePlayServicesを導入してください。

## メソッド数の上限が64Kの仕様について

Androidアプリでは、保持できるメソッド数に64K(65536)の上限があり、超えるとビルドエラーが発生します。<br>
そのため多くのメソッドを有しているGooglePlayServicesを、広告IDの取得のためだけに導入されている場合は<br>
広告IDの取得を目的に切り出された以下のライブラリをお使いください。<br>
F.O.X SDKではGooglePlayServicesを広告IDの取得を目的に利用しています。

```
dependencies {
	compile 'com.google.android.gms:play-services-ads:9.4.0'
}
```

> ※ メソッド数の上限が64Kの仕様についての詳細<br>
[Configure Apps with Over 64K Methods | Android Developers](https://developer.android.com/studio/build/multidex.html)

> ※ Google Play servicesの最新のバージョンはAndroidのデベロッパーサイトにて確認するようにしてください。<br>
[Google Play Services | Android Developers](https://developer.android.com/google/play-services/index.html)


## Google Play Servicesを利用するための設定

#### AndroidManifest.xmlの編集

Google Play Servicesを利用するために下記の設定をAndroidManifest.xmlの<application>タグ内に記述します。

```xml
<meta-data
    android:name="com.google.android.gms.version"
    android:value="@integer/google_play_services_version" />
```

#### Proguardの設定

Proguardを利用して難読化している場合は、以下の設定を追加してください。

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
[Android TOPへ](/lang/ja/doc/integration/android/README.md)
