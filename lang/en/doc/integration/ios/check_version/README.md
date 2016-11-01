## （Option）Distribution of management corresponding to handle version registered on administration screen.

It is able to distribute management corresponding to whether the version registered on FOX's administration screen and version of application which is in process are matched, or not. By using this function, for example, it is possible to use and control the distribution of managing release and testing version and conduct particular management on only testing version. Implementing this function is not mandatory.

By the way bandle version F.O.X uses is the value which is equivalent to CFBundleShortVersionString.


For distributing management corresponding to bundle version, use checkVersionWithDelegate:method. After sending the inquiry about checking version to F.O.X server and receiving the result,didLoadVersion() is called. Please implement the delegate methoddidLoadVersion(). When calling didLoadVersion(), the result, whether the bundle version registered on administration screen matches the version of application which is in process, or not, gives to the argument. If matched, respond ”YES”. If not, respond ”NO”.

Below, it is the example of implementing this function. Call checkVersionWithDelegate()method and send the inquiry to the server.

```cs
FoxPlugin.setListenerGameObject(this.gameObject.name);FoxPlugin.checkVersionWithDelegate();
```

Implement didLoadVersion() which is delegate method.

```cs
public void didLoadVersion(string result){	// The note of management in the case of not matched (for example testing version).
	if (result=="NO") {		....	}}
```

> For load reduction, the number of sending inquiry to F.O.X server by this method is limited to 5 times at each version for 1 client. After exceeding 5 times, inquiry will not be sent to the server and didLoadVersion() is not called. By updating bundle version, it is able to send inquiry to the server with the maximum 5 times.

---
[iOS TOP](../README.md)
