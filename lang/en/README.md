# About the Force Operation X

Force Operation X (Hereinafter referred to as F.O.X) is total solution platform optimizing the ad effectiveness on Smartphones. This platform provides not only a measurement of app downloads and user actions, but also helps to maximize the price-performance ratio of your app's promotional campaign through measurement of effect based on users' behavioral characteristics.

This document serves as a guide for correct installation of F.O.X. SDK in order to maximize the ad effectiveness of Smartphone applications.

## Index

* **[1. Installation](#install_sdk)**
	* [SDK downloads](https://github.com/cyber-z/public-fox-unity-sdk/releases)
  * [Integrating the Unity plugin](./doc/integration/README.md)
  * [Setting up iOS Project](./doc/integration/ios/README.md)
  * [Setting up Android Project](./doc/integration/android/README.md)
* **[2. Tracking app downloads](#tracking_install)**
* **[3. Implementing LTV tracking](#tracking_ltv)**
	* [The sendLtv() method](./doc/send_ltv_conversion/README.md)
* **[4. Tracking app access](#tracking_analytics)**
	* [Event tracking based on app access tracking](./doc/analytics_event/README.md)
* **[5. Testing the setup](#integration_test)**
  * [Test for Re-engagement tracking](./doc/reengagement_test/README.md)
* **[6. Implementation of other features](#other_function)**
  * [Implementing push notifications](./doc/notify/README.md)
  * [Implementing Opt-out](./doc/optout/README.md)
* **[7. Troubleshooting](#trouble_shooting)**

## What is F.O.X SDK?

Including F.O.X SDK into an app enables the following features.

* **Tracking app downloads**

Tracking then number of app downloads separately for each ad campaign.

* **LTV measurement**

Measurement of Life Time Value (LTV) for each ad campaign. Primary conversion points are member registration, completion of a tutorial, and billing. F.O.X also measures the registration rate, the billing rate, and the billing amount for each ad campaign.

* **Tracking app access**

Comparison of ad driven user access, and non-ad driven user access. It is also possible to measure the number of app launches, number of unique users (DAU/MAU), persistency rate, etc.

* **Push notifications**

Using the data collected by F.O.X, it is possible to send push notifications to a specific group of users. For example, it is possible to send notifications to users who installed the app through a particular ad campaign.

* **Advertisement distribution**

It is possible to display ads in the app. Implementation of this feature may be omitted if not needed.

<div id="install_sdk"></div>
## 1. Installation

Please download the latest version of SDK from the following page.

[SDK release page](https://github.com/cyber-z/public-fox-unity-sdk/releases)

Already using F.O.X? Get the latest version here [updating to latest version](./doc/update/README.md).

Extract the downloaded SDK　`"FOX_UnityPlugin_<VERSION>.zip"`, and copy it into your project.

[Integrating the Unity plugin](./doc/integration/README.md)

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
## 3. Implementing LTV tracking

By implementing LTV measurements at arbitrary conversion points such as member registration, completion of a tutorial, item purchase, etc., it is possible to measure the Life Time Value of each ad campaign. This step can be skipped if LTV measurement is not necessary.

The LTV measurement code should be included in the script that runs after the conversion. For example, for tracking  member registration or item purchase, the LTV measurement code can be included in the callback of member registration or item purchase function. Please note that implementation might change with the scripting language used (C# or JavaScript).

```cs
FoxPlugin.sendLtv(CONVERSION POINT ID);
```

In order to perform LTV measurement, you need to specify a unique ID for each conversion point. Pass the conversion point ID as a parameter to the sendLtv() method.

When tracking item purchase, specify the price of the item before calling the sendLtv() method as shown below.

```cs
// ...
FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");
FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "20");
FoxPlugin.sendLtv(LTV POINT ID);
```

> When using JavaScript, replace 'FoxPlugin' with 'FoxPluginJS'.

* [More on sendLtv()](./doc/send_ltv_conversion/README.md)

<div id="tracking_analytics"></div>
## 4. Tracking app access

F.O.X allows you to compare the ad-driven app downloads and non ad-driven app downloads. Also, it is possible to measure the number of app launches, number of unique users (DAU/MAU), persistency rate, etc. This step can skipped if app access tracking is not needed.

To implement app access tracking, follow any of the following methods.

* Using the script provided
* Writing the code yourself

### [Using the script provided]

Drag & drop "Plugins/FoxAnalyticsSession.cs" into Main Camera. This script will perform the tracking whenever the app is launched or is resumed from the background.

> If you have multiple scenes in your project, it is necessary to drag & drop the script in all the Main Cameras. If the app is resumed from background to a scene that doesn't have the script included, the tracking accuracy would decline.

### [Writing the code yourself]

Call the following method at the starting point of the scene.

```cs
FoxPlugin.sendStartSession();
```

> If you have multiple scenes in the project, call the above method in all the scenes.


* **Purchase event measurement using app access tracking**
Refer to the following link if you want to track item purchases using app access tracking.

[Event tracking based on app access tracking](./doc/analytics_event/README.md)

<div id="integration_test"></div>
## 5. Testing the setup

Before releasing the app to the market, please do ample testing of your app with the SDK, and make sure that there are no problems with the functioning of the app.

App download tracking is performed only once on first run of the app. If you want to test app download tracking again, uninstall the app, and then install it again.

* **Test procedure**

1. Uninstall the test app from the test device if already installed.
1. Clear all cookie from the default browser of the test device.
1. Click on the test URL issued by FOX.
1. URL redirects to the market.
1. Install the app on the test device.<br />
1. Run the app, it will launch the browser on first run.<br />
If the browser doesn't launch, there is something wrong with your setup. Make sure you have followed the steps correctly as shown in this guide. If you are unable to fix the setup, please contact us.
1. Navigate to a screen containing an LTV conversion point.<br />
1. Exit the app, also kill it from the background.<br />
1. Launch the app again.<br />

Please inform us the timestamp of the steps 3, 6, 7, and 9. We will check if the measurements were performed correctly. If we find no issues, the test is completed.

>Make sure you send the request to the test URL using the default browser of the test device. The tracking will fail if you open the test URL inside the Web View of, say, an email app or a QR code reader app.


[Test for re-engagement tracking](./doc/reengagement_test/README.md)


<div id="other_function"></div>
## 6.  Implementation of other features

* [Implementing push notification](./doc/notify/README.md)

* [Implementing opt-out](./doc/optout/README.md)


<div id="trouble_shooting"></div>
## 7. Troubleshooting（Collection of troubles so far）

### 7.1.Transition from browser to the app fails as the URL scheme was not set

After launching the browser to perform app download tracking using cookies, URL scheme is necessary to transition back to the app. For this reason, setting the correct URL scheme is necessary. If the app is released without setting the correct URL scheme, the transition from browser to the app will fail.

### 7.2. Transition from browser to the app fails as the URL scheme includes uppercase letters or symbols

Depending on the environment, it might not be possible to distinguish between uppercase and lowercase letters causing the transition to fail. Please use only lowercase alphanumeric characters in the URL scheme.

### 7.3. URL scheme clashes with that of another app causing the other app to be launched from the browser

In iOS, if there are multiple apps with the same URL scheme, it is not possible to determine which app will be launched. In order to avoid this, make sure your URL scheme is complex enough to prevent it from clashing with that of other apps.

### 7.4. The measurements were not made correctly when running an aggressive ad campaign to acquire a large number of users in a short period of time

iOS forcibly closes the app when the main thread is blocked for longer than a certain time of time on app startup. Do not use synchronous communication with the server on main thread. As a result of acquiring lots of users in a short time period using something like reward ads, the requests to the server might rise making the communication slower. In such a case, if the communication is performed synchronously on main thread, the application might block the main thread for longer than allowed causing the OS to kill the app forcibly. This might lead to incorrect measurements.

This can be tested using the following steps.

iOS「Settings」→「Developer」→「NETWORK LINK CONDITIONER」

* Turn on 'Enable'
* Check 'Very Bad Network'


### 7.5. The number of app downloads reported by F.O.X is larger than the number of downloads reported by Google Play Developer Console

We use a combination of methods to detect multiple installs on same device. If the settings are set in a way that doesn't allow accurate detection of multiple installs on same device, F.O.X would consider each install on the same device as a new install.

In order to increase the accuracy of detecting multiple installs on same device, follow the following guides.

* [Including Google Play Services SDK to use ad ID](./doc/integration/android/google_play_services/README.md)

* [（Optional）Duplicate install detection using external storage](/lang/en/doc/integration/android/external_storage/README.md)

* [（Optional）Using android M's auto backup feature](./doc/integration/android/auto_backup/README.md)

---
[TOP](/README.md)
