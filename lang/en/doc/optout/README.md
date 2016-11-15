## (Optional) Implementation of opt-out

It is possible to give users the ability to opt-out of targeted advertising by ad companies. If the user selects to opt-out when shown the privacy policy and terms of use, the user won't be included in any measurement or tracking results, and FOX will also inform the ad companies about that particular user's wish to opt-out.

If you want to enable opt-out feature in your app, call the setOutput() method before calling sendConversion() as shown below.

```cs// In case of user's selecting opt out, enable setOptoutif(user.optout) {	FoxPlugin.setOptout(true);}
FoxPlugin.sendConversion("default");
```

---
[TOP](/lang/en/README.md)
