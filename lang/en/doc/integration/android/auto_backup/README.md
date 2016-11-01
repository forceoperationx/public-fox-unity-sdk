## The improvement of deduplication function using auto backup function

By using auto backup which is added from Android M(6.0), it is able to improve deduplication function. Still, this correspondence is arbitrary.

## Terms of use

* It is the case of restoration of backup data from Android M terminal to more above than Android M terminal.
* It is necessary to have the same Google account when backup and restoration.
* When moving data, like following pictures, it is mandatory that setting of terminal is available by users.

![Setting screen](./img01.png)

## Setting

　When using , it is necessary to set according to the condition of application's backup setting file.

> [Reference setting](https://developer.android.com/training/backup/autosyncapi.html)

**[The case of specifying application date for backup individually]**

　Please set so that following file can be included to be subject to backup.

```
<include domain="file" path="__ADMAGE_RANDOM_DEVICE_ID__" />
```

**[The case of conducting back up data of all application]**

　In the case of setting that data of application is subject to backup, please set that following files are excluded.

```
<exclude domain="file" path="__ADMAGE_WEB_CONVERSION_COMPLETED__" />
<exclude domain="file" path="__ADMAGE_APP_CONVERSION_COMPLETED__" />
```

---
[Android TOP](/lang/ja/doc/integration/android/README.md)
