## Configuration required to make multiple INSTALL_REFERRER receivers coexist

It is not possible to define more than one receiver class corresponding to "com.android.vending.INSTALL_REFERRER".

In case you are using another SDK that already defines a class corresponding to "com.android.vending.INSTALL_REFERRER", it is possible make the receiver classes coexist by calling the other receiver from the receiver class offered by FOX SDK.

### Making two INSTALL_REFERRER receivers coexist

Add the following snippet to AndroidManifest.xml.

```xml
<!-- Receiver specifies the FOX SDK's receiver class -->
<receiver
	android:name="jp.appAdForce.android.InstallReceiver"
	android:exported="true">
	<intent-filter>
		<action android:name="com.android.vending.INSTALL_REFERRER" />
	</intent-filter>
</receiver>

<!-- The other receiver class to be called from the FOX SDK's receiver class is mentioned in the meta-data -->
<meta-data
	android:name="APPADFORCE_FORWARD_RECEIVER"
	android:value="com.example.InstallReceiver" />
```

### Making three or more receiver classes coexist

When incorporating three or more receivers, define a main receiver class that calls all other receiver classes one by one.

Sample implementation is shown below.

* MyReceiver Class（You can name it whatever you want, of course）

```java
public class MyReceiver extends BroadcastReceiver {
	// F.O.X's INSTALL_REFERRER receiver
   jp.appAdForce.android.InstallReceiver foxReceiver;

   //  Third party INSTALL_REFERRER receivers
   jp.co.android.sample.InstallReceiver thirdPartyReceiver;
   jp.co.android.sample.InstallReceiver2 thirdPartyReceiver2;

   public MyReceiver() {
      foxReceiver = new jp.appAdForce.android.InstallReceiver();
      thirdPartyReceiver = new jp.co.android.sample.InstallReceiver();
      thirdPartyReceiver2 = jp.co.android.sample.InstallReceiver2();
   }
   @Override
   public void onReceive(Context context, Intent intent){
       foxReceiver.onReceive(context, intent);

       thirdPartyReceiver.onReceive(context, intent);

       thirdPartyReceiver2.onReceive(context, intent);
   }
}
```

* AndroidManifest.xml

```xml
<receiver
       android:name="jp.co.sample.android.receiver.MyReceiver"
       android:exported="true" >
       <intent-filter>
            <action android:name="com.android.vending.INSTALL_REFERRER" />
       </intent-filter>
</receiver>
```

---
[Android TOP](/lang/en/doc/integration/android/README.md)
