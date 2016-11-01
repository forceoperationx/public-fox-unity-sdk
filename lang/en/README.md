# About the Force Operation X

Force Operation X (Hereinafter referred to as F.O.X) is total solution platform optimizing the ad effectiveness on Smartphones. This platform provides not only a measurement of app downloads and user actions, but also helps to maximize the price-performance ratio of your app's promotional campaign through measurement of effect based on users' behavioral characteristics.

This document serves as a guide for correct installation of F.O.X. SDK in order to maximize the ad effectiveness of Smartphone applications.

## Index

* **[1. Install](#install_sdk)**
	* [SDK downloads](https://github.com/cyber-z/public-fox-unity-sdk/releases)
  * [How to implement Unity plug-in](./doc/integration/README.md)
  * [iOS project setup guide](./doc/integration/ios/README.md)
  * [Android project setup guide](./doc/integration/android/README.md)
* **[2. Implementing app installation measurement](#tracking_install)**
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

* **Measurement of app downloads**

Measurement of number of app downloads separately for each ad campaign.

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

In the case of already implementing SDK into application, please refer to [updating the latest version](./doc/update/README.md).

Please decompress the downloaded SDK　`"FOX_UnityPlugin_<VERSION>.zip"`,and put into the projects of application.

[How to implement Unity plug-in](./doc/integration/README.md)

### Setting of each OS

* [Setting of iOS project](./doc/integration/ios/README.md)
* [Setting of Android project](./doc/integration/android/README.md)


<div id="tracking_install"></div>
## 2. The implementation of installation measurement

By implementing install measurements for first time, it is able to start the effectiveness measurements of advertisement. Please implement in a order following.

* Setting Installing measurement by GUI(inspector)
* Write the code


### Implementation of installing measurement by GUI(Inspector)

If there is the object that is read at only once in activation, editing by GUI(Inspector) is available.

Example) Using Inspector of Main Camera, perform installing measurement.

1. Drag & drop "Plug/FoxPlugin.cs" to Main Camera.
2. On the inspector of Main Camera, specify the string default to Url variable of Plugin script.


### Implementation of the installing measurement when you write the code

When you write the installation measurement process in the script without using GUI(Inspector),  call FoxPlugin.sendConversion from scripts that is run at startup.

For iOS, when going back to applications from starting browsers at first starting, dialogue will be exported.
In F.O.X SDK, by starting “SFSafariViewController”, which is a new WebView form released from iOS9, at first staring and measuring, it is able to prevent from the decline of usability caused by displaying dialogue.

```cs
FoxPlugin.sendConversion("default");
```

For argument of sendConversion, input the string "default" of the above usually.

To make transition to certain URL or to make URL automatically by apps, set string of URL.

```cs
FoxPlugin.sendConversion("http://yourhost.com/yourpage.html");
```

The advertiser terminal ID can be given to the second argument of sendConversion method.<br>
For example, it can be used at the point of activation  SDK makes UUID, if you want to manage that with the results of the first activation.

```cs
FoxPlugin.sendConversion("default", "your unique id");
```

> ※If default is specified, standard simple sample page will appear first, but we will set transition destination URL or HTML page on management screen of F.O.X later. By the time of release to market, please notice us the name of URL scheme to return from transition destination page to app.

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

In order to increase the accuracy of duplication install detection, make the following settings.

* [The implementation of Google Play Services SDK to use advertisement ID](./doc/integration/android/google_play_services/README.md)

* [（Option）Deduplication setting using external storage](/lang/en/doc/integration/android/external_storage/README.md)

* [（Option）Usage of Android M auto backup function](./doc/integration/android/auto_backup/README.md)

---
[TOP](/README.md)
