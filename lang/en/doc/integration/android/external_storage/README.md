## Configuring duplicate entry prevention using external storage

By preserving the unique user ID that the SDK generated on first run of the app on local storage or SD card, reinstallations can tracked.

This configuration can be skipped, however, we recommend using this feature as it helps improve the accuracy of duplicate entry detection.

### Setting up the necessary permissions
Add the following permissions necessary for reading and writing a file to external storage in the &lt;manifest&gt; tag of AndroidManifest.xml.

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" /><uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

If the above permissions are provided, the unique user ID is saved in the following file by default.

```
Environment.getExternalStorageDirectory().getPath() / Application's package name / __FOX_XUNIQ__
```

### （Optional）Change of preserved directory and file name

If you want to change the directory or the filename then add the folllowing meta-data to AndroidManifest.xml.

```xml
<meta-data
	android:name="APPADFORCE_ID_DIR"
	android:value="Directory name" />
<meta-data
	android:name="APPADFORCE_ID_FILE"
	android:value="File name" />
```

> The directory will be created under Environment.getExternalStorageDirectory().getPath(). The path returned by Environment.getExternalStorageDirectory().getPath() is different for different devices.

> If only `APPADFORCE_ID_FILE` is specified, then the file will be created in app's package name directory.

> If only `APPADFORCE_ID_DIR` is specified, then __FOX_XUNIQ__ file will be created under the specified directory.
Providing `APPADFORCE_ID_DIR` and `APPADFORCE_ID_FILE` is not needed usually.


###  Sample configuration

Here is a sample snippet of AndroidManifest.xml.

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

For the above snippet, the unique user ID will be saved in the following file.

```
Environment.getExternalStorageDirectory().getPath() / fox_id_dir / fox_id_file
```

### Preventing FOX SDK from accessing external storage

If you want to prevent FOX from accessing the external storage, add the `APPADFORCE_USE_EXTERNAL_STORAGE` meta-data and set its value to 0 as shown below.
```xml
<meta-data
	android:name="APPADFORCE_USE_EXTERNAL_STORAGE"
	android:value="0" />
```

If the above meta-data is added, FOX will not access the external storage. Howerever, doing so will reduce the accuracy of installation tracking as reinstalls will be considered as new installs.

### Regarding Android M (6.0)

When doing somethign that requires a `dangerous` protection level permission, the OS will ask for user approval. If the user doesn't permit, the SDK wouldn't be able to access the external storage and reinstall tracking will fail. `READ_EXTERNAL_STORAGE` and `WRITE_EXTERNAL_STORAGE` permissions have `dangerous` protection level, so it is necessary to perform the implementation in a way that fetches the permission from the user.

* [Sample implementation](https://developer.android.com/training/permissions/requesting.html#perm-request)

---
[Android TOP](/lang/en/doc/integration/android/README.md)
