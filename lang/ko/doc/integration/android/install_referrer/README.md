[TOP](../../../../README.md)　>　[Unity 플러그인의 도입 절차](../../README.md)　>　[Android 프로젝트의 설정](../README.md)　>　**복수의 INSTALL_REFERRER레시버를 설정**

---

## 복수의 INSTALL_REFERRER레시버를 공존시키기 위한 설정

"com.android.vending.INSTALL_REFERRER"에 대한 receiver클래스는 하나 밖에 정의 할 수 없습니다.

F.O.X 이외의 SDK 등 이미 "com.android.vending.INSTALL_REFERRER"에 대한 receiver클래스가 정의되어있는 경우에는 F.O.X SDK가 제공하고 있는 receiver클래스에서 다른 receiver클래스를 호출하여 공존시키는것이 가능합니다.

AndroidManifest.xml을 편집하고 다음 설정을 추가하십시오.

```xml
<!-- 레시버 클래스는 F.O.X SDK의 클래스를 지정합니다. -->
<receiver
	android:name="co.cyberz.fox.FoxInstallReceiver"
	android:exported="true">
	<intent-filter>
		<action android:name="com.android.vending.INSTALL_REFERRER" />
	</intent-filter>
</receiver>

<!-- F.O.X SDK에서 호출하고 싶은 레시버 클래스 정보를 meta-data로 기술합니다. -->
<meta-data
		android:name="APPADFORCE_FORWARD_RECEIVER"
		android:value="com.example.InstallReceiver1|com.example.InstallReceiver2|com.example.InstallReceiver3" />
```

> `APPADFORCE_FORWARD_RECEIVER`에 지정한 클래스는 패키지와 더불어 지정하여 주십시오. 또는 `|`(파이프)로 구별하여 복수의 클래스를 지정하는 것이 가능합니다.

> Proguard를 이용하는 경우 `APPADFORCE_FORWARD_RECEIVER`에 지정한 클래스는 -keep지정으로 클래스명이 변경되지 않도록 하여 주십시오<br>
Proguard의 대상이되는 경우 F.O.X SDK를 찾을수 없게되어 정상적으로 동작하지 않으므로 주의하여 주십시오.


---
[戻る](../README.md)

[TOP](../../../../README.md)
