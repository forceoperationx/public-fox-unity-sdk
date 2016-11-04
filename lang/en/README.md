# About the Force Operation X

Force Operation X (Hereinafter referred to as F.O.X) is total solution platform optimizing the ad effectiveness on Smartphones. This platform provides not only a measurement of app downloads and user actions, but also helps to maximize the price-performance ratio of your app's promotional campaign through measurement of effect based on users' behavioral characteristics.

This document serves as a guide for correct installation of F.O.X. SDK in order to maximize the ad effectiveness of Smartphone applications.

## Index

* **[1. Installation](#install_sdk)**
	* [SDK downloads](https://github.com/cyber-z/public-fox-unity-sdk/releases)
  * [How to implement Unity plug-in](./doc/integration/README.md)
  * [iOS project setup guide](./doc/integration/ios/README.md)
  * [Android project setup guide](./doc/integration/android/README.md)
* **[2. Tracking app downloads](#tracking_install)**
* **[3. Implementing LTV measurement](#tracking_ltv)**
	* [The sendLtv() method](./doc/send_ltv_conversion/README.md)
* **[4. Implementing app access analysis](#tracking_analytics)**
	* [Event measurement using app access analysis](./doc/analytics_event/README.md)
* **[5. Testing the setup](#integration_test)**
  * [Testing re-engagement measurement](./doc/reengagement_test/README.md)
* **[6. Implementing other features](#other_function)**
  * [Implementing push notification](./doc/notify/README.md)
  * [Implementing Opt-out](./doc/optout/README.md)
* **[7. Troubleshooting](#trouble_shooting)**

## What is F.O.X SDK?

Including F.O.X SDK into an app enables the following features.

* **Tracking app downloads**

Tracking then number of app downloads separately for each ad campaign.

* **LTV measurement**

Measurement of Life Time Value (LTV) for each ad campaign. Primary conversion points are member registration, completion of a tutorial, and billing. F.O.X also measures the registration rate, the billing rate, and the billing amount for each ad campaign.

* **App Access Analysis**

Comparison of ad driven user access, and non-ad driven user access. It is also possible to measure the number of app launches, number of unique users (DAU/MAU), persistency rate etc.

* **Push notifications**

Using the data collected by F.O.X, it is possible to send push notifications to a specific group of users. For example, it is possible to send notifications to users who installed the app through a particular ad campaign.

* **Advertisement distribution**

It is possible to display ads in the app. Implementation of this feature may be omitted if not needed.

<div id="install_sdk"></div>
## 1. Installation

Please download the latest version of SDK from the following page.

[SDK release page](https://github.com/cyber-z/public-fox-unity-sdk/releases)

Already using F.O.X? Get the latest version here [updating the latest version](./doc/update/README.md).

Extract the downloaded SDK　`"FOX_UnityPlugin_<VERSION>.zip"`, and copy it into your project.

[How to implement Unity plug-in](./doc/integration/README.md)

### iOS and Android setup guide

* [iOS](./doc/integration/ios/README.md)
* [Android](./doc/integration/android/README.md)


<div id="tracking_install"></div>
## 2. Tracking app downloads

By implementing app download tracking, it is possible to measure the effectiveness of your ad campaign. To setup app download tracking, follow one of the methods mentioned below.

* Setting up app download tracking using GUI (inspector)
* Setting up app download tracking using code

### Setting up app download tracking using GUI (inspector)

If there exists an object that is loaded only once on app startup, then changes using GUI (Inspector) become possible.

Example) Tracking app downloads using the Inspector of the Main camera.

1. Drag & drop "Plug/FoxPlugin.cs" into the Main Camera.
2. Using the inspector of the Main Camera, set the 'Url' variable of the Fox plugin script to 'default'.

### Setting up app download tracking using code

To track app downloads without using GUI (Inspector), call 'FoxPlugin.sendConversion' method from a script that runs on app startup.

From iOS 9 onwards, when running the app for the first time, a dialog box pops up when returning from the browser to the app. In F.O.X SDK, the app download tracking is performed using the new "SFSafariViewController" WebView format available in iOS 9 and later, which prevents user experience from degrading due to the dialog box.

```cs
FoxPlugin.sendConversion("default");
```

Pass the string "default" as a parameter to the sendConversion method as shown above.

To redirect to a certain URL or to generate the URL dynamically inside the app, pass the URL in a string as shown below.

```cs
FoxPlugin.sendConversion("http://yourhost.com/yourpage.html");
```

You can also pass the UUID of the user as a second argument to the sendConversion method.<br>
This is useful if you want the UUID information along with the first run details of the app.

```cs
FoxPlugin.sendConversion("default", "your unique id");
```

> ※If 'default' is passed as an argument to the method, a standard sample page will be displayed first, but this can be changed to a specific HTML page or a URL from the F.O.X developer console. To return to the app from this page, URL scheme of the app is necessary. Please inform us about your app' URL scheme before releasing the app to the market.

<div id="tracking_ltv"></div>
## 3. Implementation of LTV management

By implementing LTV measurements at arbitrary conversion points such as membership registration, completion of tutorial, billing and etc, It measures Life Time Value for each advertisements of influx sources. In the case that LTV measurement is not necessary, it is able to omit this implementation.

Editting source writes process on script that is run after outcome is achieved. For example, at account measurement after membership registration or in-app billing, describe the LTV measurement processing in callback after  registration and billing processing execution.


```cs
FoxPlugin.sendLtv(LTV POINT ID);
```

In order to perform measure the LTV measurement, you need to specify the outcome point ID that identifies each achievement point. Please specify the issued ID in the argument of the sendLtv.

In the case of performing the accounting measurement, please specify the billing amount and the currency code in the following manner at the point where the charging is complete.

```cs
// ...
FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");
FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "20");
FoxPlugin.sendLtv(LTV POINT ID);
```

> When you edit by Javascript, read "FoxPlugin" in the text to "FoxPluginS"

* [Detail of sendLtv ](./doc/send_ltv_conversion/README.md)


<div id="tracking_analytics"></div>
## 4. Implementation of the access analysis

You can measure comparison of Installation number of naturally flow and advertising flows, the number of the of activating application and unique users (DAU / MAU),  the ongoing rate and so on. When the access analysis is not required, you can omit the implementation of this item.

To perform access analysis, make any procedure in the following.

* Use script
* Write the code

### [When you use script]

Drag & drop "Plugins/FoxAnalyticsSession.cs" to Main Camera". Perform At the point of activating apps or returning from background, perform session start measurement.

> If multiple Scene exist in project, set  measurement point on all MainCamera. If there are return from background as unset Scene is displayed, measurement will lose accuracy.


### [Incase of writing code]

Implement following method at start point of Scene.

```cs
FoxPlugin.sendStartSession();
```

> If multiple Scene exist in project, implement on all orbital points.


* **Purchase event measurement by access analysis**
Refer to following link if you want to carry on accounting measurement by accounting measurement access analysis by access analysis.

[Event measurement by access analysis](./doc/analytics_event/README.md)

<div id="integration_test"></div>
## 5. Implementation of the communication test

Until the application to market, test enough in a state where the the SDK has been introduced, and  make sure that there is no problem in the operation of the application.

Communication of Installation measurement is performed only  once after startup. If you want to do effect measurement test next, uninstall the application, please go from the installation again.

* **Test procedure**

1. Uninstall test app if it has been installed in the testing terminal.
1. Delete the Cookie of the default browser of the testing terminal.
1. Click the test URL that issued from our company
1. Redirected to the Market
1. Install a test application to the testing terminal<br />
1. Activate the app, the browser will start-up<br />
If the browser does not start, that means setting has not been carried out correctly. Review the settings, and if you do not see any problem, please contact us.
1. Screen transition to LTV point<br />
1. Quit the app, also removed from the background<br />
1. Activate the app again<br />

Please tell the time of 3,6,7,9 to us. We will see if measurement has been successfully performed. If there are no problems in our confirmation, the test will be completed.

> Make sure that test URL will be requested on default browser of the terminal. The measurement will not be performed if you transit by Web View in the app such as mail app or QR code reader.

> When you click on a test URL, there is a case where error dialog is displayed because there is no transit destination, but this is not a problem in the communication test.


[Test procedure as performed reengagement measurement.](./doc/reengagement_test/README.md)


<div id="other_function"></div>
## 6.  The implementation of other functions

* [The implementation of push notification](./doc/notify/README.md)

* [The implementation of opt-out](./doc/optout/README.md)


<div id="trouble_shooting"></div>
## 7. Please confirm（Collection of troubles so far）

### 7.1. It is released without setting of URL scheme and it does not transit to application from browser.

After starting external browsers to conduct Cookie measurement, it is necessary to go back to original screen by using URL scheme and transits to application. At this moment, it is necessary to set original URL scheme. If releasing without setting of URL scheme, it does not conduct such a transition.

### 7.2. URL scheme includes capital letters and small letters and it does not normally conduct transition to application

Depending on the environment, the capital letters and small letters of URL scheme are not distinctive and it does not conduct transition of URL scheme as usual. Please set the URL scheme in all small letters and alphanumeric.


### 7.3. The setting of URL scheme is same as application made from other companies and it opens the application from browsers.

In iOS, in the case of setting same URL scheme in several application, nobody knows which application is started. It does not start specific application for sure, so URL scheme should have some complexity which are unique to application made from other companies.

### 7.4. When operating promotion to acquire tons of users in short time, it did not measure.

iOS forcibly close the application when main thread is blocked for longer than certain time at staring application. Please do not synchronous communication to server on main thread, such as initialization at starting. As a result of acquiring tons of users in short time by reward advertisement, access to the server is concentrated and normal response is getting worse and worse. Then, it takes much time to start application and there is a case that it is forcibly closed and does not measure advertisement effectiveness when starting.

By following procedures below, it is able to practice test in the condition, so please confirm that application is normally started in following setting.

iOS「Setting」→「Developer」→「NETWORK LINK CONDITIONER」

* 「Enable」is on.
* Check「Very Bad Network」


### 7.5. The value of installation confirmed in F.O.X is bigger than the number of Google Play Developer Console

In F.O.X, by combining some formulations, conduct duplication install detection for terminal. WIthout the setting not conducting the detection, F.O.X judges that it is a new installation at every time of reinstallation even in same terminal.

重複検知の精度を向上するために、以下の設定を行ってください。

* [The implementation of Google Play Services SDK to use advertisement ID](./doc/integration/android/google_play_services/README.md)

* [（Option）Deduplication setting using external storage](/lang/en/doc/integration/android/external_storage/README.md)

* [（Option）Usage of Android M auto backup function](./doc/integration/android/auto_backup/README.md)

---
[TOP](/README.md)
