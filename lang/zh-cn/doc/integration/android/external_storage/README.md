[TOP](../../../../README.md)　>　[Unityプラグインの導入手順](../../README.md)　>　[Android プロジェクトの詳細設定](../README.md)　>　**外部ストレージを利用した重複排除設定**

---

# 利用外部储存来优化排除重复（不推荐）

**重要**

**Android Q （API29）之后访问外部储存的机制有很大变化，Target设定为API Level 29之后的APP将无法使用本功能。同时，F.O.X也计划将本功能移除。**

**关于今后的排除重复设定，请参照[使用自动备份功能](../auto_backup/README.md)。**

**如果想要在Android Q中也暂时继续使用本功能，请进行一下1或者2的设定。详细参考（[Opt out of filtered view](https://developer.android.com/preview/privacy/scoped-storage#opt-out-of-filtered-view)）。**

**1. 将Target设置为Android 9 （API Level 28）以前。**

**2. 将Target设置为Android Q的情况下，在manifest中设置`requestLegacyExternalStorage=true`。**

---

APP首次启动时，SDK生成的识别ID保存在本地储存或外部SD卡中，可以在APP再次安装时利用该ID进行重复判定。

本设置为可选项目，但为了提高APP再次安装时重复检查的精度，建议使用。

### 权限设置

在AndroidManifest.xml的<manifest>标签中添加读写外部储存必要的权限设置。

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

已设置上述权限的情况，ID识别文件会保存在以下路径。

```
用Environment.getExternalStorageDirectory().getPath()可获取路径/APP的Package名/__FOX_XUNIQ__
```

### （任意）修改保存目录及文件名

保存文件的目录名，一般为Package名，在<application>标签下添加以下设置可以修改目录名以及文件名。

```xml
<meta-data
	android:name="APPADFORCE_ID_DIR"
	android:value="任意目录名" />
<meta-data
	android:name="APPADFORCE_ID_FILE"
	android:value="任意文件名" />
```

> 指定其他目录名和文件名时，也是在 Environment.getExternalStorageDirectory().getPath()的返回值路径下生成。Environment.getExternalStorageDirectory().getPath()的返回值根据终端和OS版本的不同而有所差异。

> 未指定`APPADFORCE_ID_DIR`(任意目录名)，仅指定文件名时，会生成APP的Package名的目录，并在该路径下保存文件。<br>

> ※ 未指定`APPADFORCE_ID_FILE`(任意文件名)，仅指定目录的情况时，会生成指定的目录名，并在该路径下保存为"__FOX_XUNIQ__"。<br>
通常不需要设置。


### 设置案例

以下为AndroidManifest.xml的设置案例。

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

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

在上述案例中，文件保存为以下路径。

```
用Environment.getExternalStorageDirectory().getPath()获取的路径/fox_id_dir/fox_id_file
```

### 停止使用外部储存

想要通过Force Operation X SDK停止访问外部储存时，请在AndroidManifest.xml中添加APPADFORCE_USE_EXTERNAL_STORAGE的设置。

```xml
<meta-data
	android:name="APPADFORCE_USE_EXTERNAL_STORAGE"
	android:value="0" />
```

通过本项设置可以停止对外部储存的记录，但删除APP时会清空数据，可能导致无法进行准确计测。

### 关于Android M(6.0)的注意点

使用protectionLevel为dangerous且需要权限的功能时，必须得到用户许可。 用户拒绝时，数据将无法保存在外部存储区域进而无法进行排除重复设置。之前说到的`READ_EXTERNAL_STORAGE`和`WRITE_EXTERNAL_STORAGE`也为dangerous，需获得用户许可。

> [实际安装案例(Android Developers)](https://developer.android.com/training/permissions/requesting.html#perm-request)

---
[返回](../README.md)

[Top](../../../../README.md)
