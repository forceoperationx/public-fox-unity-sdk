[TOP](../../../README.md)　>　[イベント計測の詳細](../README.md) > **エンゲージメント計測**

---

# エンゲージメント計測

エンゲージメント計測および媒体と連携したイベント計測の説明を以下に記します。

## イベント情報の通知

FoxEventのeventInfoフィールドにJson文字列を格納し、`trackEvent`イベントを実行します。

```cs
using Cyz;
...

FoxEvent e = new FoxEvent("_purchase_item", 12345);
e.eventInfo = "{'product':[{'id':'XXXX'},{'id':'XXXX'}],'hoge':'xxxxxxxxxxx'}";
Fox.trackEvent(e);
```


## ユーザー情報の通知

ユーザー情報のJson文字列を引数にsetUserInfoを呼び出します。<br>
setUserInfoにセットされたユーザー情報はtrackEventが実行されるタイミングで送信されます。

```cs
using Cyz;
...

string userInfo = "{'guid':'xxxxxxxxxxx', 'ext':{'XXXX':'XXXXX', 'XXXXX':'XXXXXXX'}}";
Fox.setUserInfo(userInfo);
```


---
[戻る](../README.md)

[トップ](../../../README.md)
