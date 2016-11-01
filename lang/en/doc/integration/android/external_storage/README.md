## Setting of deduplication using external storages

By preserving identification ID, that SDK created when first starting of application, into local storage or, SD card, it is able to conduct deduplication when reinstalling application.

This setting is not mandatory, however, it helps improving the accuracy of duplicating detection when reinstalling application, so we recommend the implementation.

### Setting of permission

Add the necessary setting of permission for reading and writing file to external storage into &lt;manifest&gt; tag of AndroidManifest.xml.

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" /><uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

In the case of setting the above permission, identification ID file is preserved into next pass.

```
Environment.getExternalStorageDirectory().getPath() / Application's package name / __FOX_XUNIQ__
```

### （Arbitraty）Change of preserved directory and file name

Preserved file's directory name is normally created by package name, however, by adding following setting into tag, it is able to change to arbitrary directory name and file name.

```xml
<meta-data
	android:name="APPADFORCE_ID_DIR"
	android:value="Preserved directory name" />
<meta-data
	android:name="APPADFORCE_ID_FILE"
	android:value="Preserved directory name" />
```

> Even in the case of specifying the arbitrary directory name and filename, it is created under the pass of the return value of  Environment.getExternalStorageDirectory().getPath(). The return value of Environment.getExternalStorageDirectory().getPath() changes depending on the terminal, ot OS version.  <br>

> In the case of specifying the arbitrary file name without specifying `APPADFORCE_ID_DIR`(arbitrary directory name),  directory with package name will be created and preserved with arbitrary file name under it. <br>

> ※ In the case of specifying directory name without specifying `APPADFORCE_ID_FILE`(arbitrary file name), directory with arbitrary name will be created, and directory with the name __FOX_XUNIQ__ will be preserved under it.<br>
 Normally, this setting is not necessary.


###  Example of setting

Example of setting AndroidManifest.xml is noted next.

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" /><uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

<application
	android:icon="@drawable/ic_launcher"
	android:label="@string/app_name" >

	<meta-data
		android:name="APPADFORCE_ID_DIR"
		android:value="fox_id_dir" />
	<meta-data
		android:name="APPADFORCE_ID_FILE"
		android:value="fox_id_file" />

</application>
```

In the case of the above example, pass of preserved file will be next name.

```
Environment.getExternalStorageDirectory().getPath() / fox_id_dir / fox_id_file
```

### Suspension of external storage

In the case of the above example, pass of preserved file will be next name.
```xml
<meta-data
	android:name="APPADFORCE_USE_EXTERNAL_STORAGE"
	android:value="0" />
```

By this setting, the record towards external storages will be stopped, however, accurate installation measurement will not be conducted because data is always initialized by deleting application.


### Caution for Android M(6.0)

Using the function requiring permission that protectionLevel is specified as dangerous, user's approval is mandatory. In the case of not approved by users, the setting of deduplication is not available because of being able to preserve data into storage domain. Even in above READ_EXTERNAL_STORAGE and WRITE_EXTERNAL_STORAGE, it specifies the dangerous, so the implementation is necessary to get the permission from users.

* [Example of the implementation](https://developer.android.com/training/permissions/requesting.html#perm-request)

---
[Android TOP](/lang/ja/doc/integration/android/README.md)
