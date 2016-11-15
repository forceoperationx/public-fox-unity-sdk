## Test for Re-engagement tracking

Before the app is released to the market, make sure to test it thoroughly after the FOX SDK is imported.

App install tracking is performed only once during the app's first launch. To test the app install tracking again, uninstall the app and install it again.

* **Test procedure**

1. Uninstall the test app if it is already installed in the testing device.
1. Delete all Cookies from the default browser of the testing device.
1. Click the test URL that is issued by FOX
1. URL redirects to market
1. Install the test app on teh device<br />
1. Launch the app, browser will be launched after the app launch<br />
If the browser doesn't launch, there is something wrong with your setup. Make sure you have followed the steps correctly as shown in this guide. If you are unable to fix the setup, please contact us.
1. Navigate to a screen containing an LTV conversion point.<br />
1. Exit the app, also kill it from the background.<br />
1. Delete the browser Cookies, click the URL for the re-engagement tracking test. See if the app launches upon clicking the URL.<br />
※ If the application does not start, please check whether correct URL scheme has been set.<br />
Review the settings, please contact us if you can't solve the issue.<br />
1. Navigate to a screen containing an LTV conversion point.<br />
1. Exit the app, but this time do not kill it from the background.
1. Delete the browser Cookie again, click the URL for the re-engagement measurement test. See if the app launches.<br />
※ If the application does not start, please check whether correct URL scheme has been set.<br />
Review the settings, please contact us if you can't solve the issue.<br />
1. The app should go to the appropriate LTV conversion point.
1. Exit the app, also kill it from the background.
1. Launch the app again.  


Please inform us the timestamp of the steps 3, 6, 7, 9, 10, 12, and 13. We will check if the measurements were performed correctly. If we find no issues, the test is completed.

>Make sure you send the request to the test URL using the default browser of the test device. The tracking will fail if you open the test URL inside the Web View of, say, an email app or a QR code reader app.

> When you click on a test URL, sometimes an error dialog is displayed when there is no transit destination, but this is not a problem for communication to server tests.

---
[TOP](/lang/en/README.md)
