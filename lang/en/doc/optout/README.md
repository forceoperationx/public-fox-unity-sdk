## (Option) Implementation of opt out

Users are enabled to select not being used for targeting Ads by advertising company. If a user selects opt out on dialog  showing privacy policy or terms of service  at activation, F.O.X notice the company that the user has selected opt out with result of measurement.
If you want to enable opt out, make setting as follows before FoxPlugin.sendConversion implemented on Implementation of Installing measurement.

```cs// In case of user's selecting opt out, enable setOptoutif(user.optout) {	FoxPlugin.setOptout(true);}
FoxPlugin.sendConversion("default");
```

---
[TOP](/lang/ja/README.md)
