[TOP](../../../../README.md)　>　[Unity插件的导入步骤](../../README.md)　>　[Android 项目的详细设置](../README.md)　>　**Google Play Services的导入**

---

# Google Play Services的导入

将Google Play Services导入项目。<br>

打开APP模块目录中的build.gradle，按以下方法，对最新Google Play services添加依存(dependencies)设置。

```
dependencies {
	compile 'com.google.android.gms:play-services:9.4.0'
}
```
> ※ GooglePlayServices导入详细请参考[`Setting Up Google Play Services | Android Developers`](https://developer.android.com/google/play-services/setup.html)中确认。


> ※ 随着ADT的Eclipse支持结束，在Eclipse开发环境的导入方法已经被废除。<br>开发环境为Eclipse的场合，请使用[Google Play Services Jar Resolver Library for Unity](https://github.com/googlesamples/unity-jar-resolver)导入GooglePlayServices。

## 关于方法数上限到64K的式样

Android APP中，可以维持的方法数上限为64K(65536)，超过上限时会发生编译错误。<br>
因此，如果仅为获取广告ID而想导入拥有众多方法的GooglePlayServices时<br>
请使用下面这个专门获取广告ID的类库。<br>
在F.O.X SDK中使用GooglePlayServices的目的只为获取广告ID。

```
dependencies {
	compile 'com.google.android.gms:play-services-ads:9.4.0'
}
```

> ※ 方法数上限64K的式样详细<br>
[Configure Apps with Over 64K Methods | Android Developers](https://developer.android.com/studio/build/multidex.html)

> ※ Google Play services的最新版本请在Android developer site中确认。<br>
[Google Play Services | Android Developers](https://developer.android.com/google/play-services/index.html)


## Google Play development program准则

Force Operation X Android SDK以Google Play development program 条约为准则。该SDK为了遵守条约，已经获取永久设备ID（IMEI、MAC adress以及AndroidID）的情况下将不会获取广告ID。2014年8月1日开始，Google Play Store上所有的更新以及发布新app时，以广告为目的的终端ID必须使用广告ID。为了遵守该条约，请执行以下步骤。

---
[Top](../../README.md)
