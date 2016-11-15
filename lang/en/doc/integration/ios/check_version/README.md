## （Optional）Separating the execution based on the Bundle version specified on FOX developer console

The execution can be separated on whether the bundle version specified on FOX developer console and the version running on the application is the same or not. Using this feature, for example, it is possible to execute test-only code by controlling the release and test bundle version on FOX developer console. Implementation of this feature can be skipped.
The bundle version used by FOX is stored in CFBundleShortVersionString.
To seperate the execution based on Bundle version, checkVersionWithDelegate() method is used. Version checking is conducted against FOX servers (i.e it is checked whether the app has the same bundle version as was specified on FOX developer console) and didLoadVersion() method is called when a response is received. Please implement the didLoadVersion() method. The result of version checking is passed as a parameter to didLoadVersion() method. If the two versions match then the parameter passed is "YES", "NO" is passed otherwise.

The implementation is shown below.

```cs
FoxPlugin.setListenerGameObject(this.gameObject.name);FoxPlugin.checkVersionWithDelegate();
```

Implement the didLoadVersion() delegate method.

```cs
public void didLoadVersion(string result){	// The note of management in the case of not matched (for example testing version).
	if (result=="NO") {		....	}}
```

> For load reduction purposes, we have limited the maximum number of possible requests to servers to 5. If the number of requests exceed 5, then the version checking cannot be performed and didLoadVersion() won't be called. This counter is reset when the bundle version is updated.

---
[iOS TOP](../README.md)
