[TOP](../../../README.md)　>　[事件计测详细](../README.md) > **一般广告计测(engagement)**

---

# 一般广告计测(engagement)

エンゲージメント計測および媒体と連携したイベント計測の説明を以下に記します。
下面针对一般广告计测以及媒体连动事件计测进行说明。

## 事件信息的通知

在FoxEvent的eventInfo字段里放置Json字符串、执行`trackEvent`事件。

```cs
using Cyz;
...

FoxEvent e = new FoxEvent("_purchase_item", 12345);
e.eventInfo = "{'product':[{'id':'XXXX'},{'id':'XXXX'}],'hoge':'xxxxxxxxxxx'}";
Fox.trackEvent(e);
```


## 用户信息的通知

把用户信息的Json字符串当做参数调用setUserInfo方法。<br>
放置在setUserInfo里的用户信息会在执行trackEvent的时机被发送出去。

```cs
using Cyz;
...
// KeyとValueが文字列の場合、ダブルクォーテーションで囲ってください。
// また、ダブルクォーテーションの前にエスケープ文字必須です。
string userInfo = "{\"XXXX\":\"xxxxxxxxxxx\", \"XXXX\":0}";
Fox.setUserInfo(userInfo);
```

---
[返回](../README.md)

[Top](../../../README.md)
