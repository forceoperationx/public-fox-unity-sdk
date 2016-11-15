## Improving duplicate entry prevention using external storage

Using the auto backup feature on Android M onwards, it is possible to improve the accuracy of duplicate entry detection. However, the accuracy can still not be guaranteed to be 100%.

## Terms of use

* This would work only on Android M and above devices using data backup.
* The Google account while making the backup and while restoring it should be the same.
* The user must have 'Back up my data', and 'Automatic restore' switched on as shown in the image below.

![Setting screen](./img01.png)

## Configuration

　When using this feature, it is necessary to configure the app properly as shown below.

> [Refer here for details](https://developer.android.com/training/backup/autosyncapi.html)

**[When explicitly specifying app's backup data]

　Please include the following file in backup

```
<include domain="file" path="__ADMAGE_RANDOM_DEVICE_ID__" />
```

**[When backing up all the app data]**

　When backing up all app data, please exclude the following two files from the backup.

```
<exclude domain="file" path="__ADMAGE_WEB_CONVERSION_COMPLETED__" />
<exclude domain="file" path="__ADMAGE_APP_CONVERSION_COMPLETED__" />
```

---
[Android TOP](/lang/en/doc/integration/android/README.md)
