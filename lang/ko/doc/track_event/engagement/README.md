[TOP](../../../README.md)　>　[이벤트 계측의 상세](../README.md) > **엔게이지멘트 계측**

---

# 엔게이지멘트 계측

엔게이지멘트 계측 및 미디어와 연계된 이벤트 계측의 설명을 아래에 기술합니다.

## 이벤트 정보의 통지

FoxEvent의 eventInfo필드에 Json문자열을 격납하여 `trackEvent`이벤트를 실행합니다.

```cs
using Cyz;
...

FoxEvent e = new FoxEvent("_purchase_item", 12345);
e.eventInfo = "{'product':[{'id':'XXXX'},{'id':'XXXX'}],'hoge':'xxxxxxxxxxx'}";
Fox.trackEvent(e);
```


## 유저 정보의 통지

유저 정보의 Json문자열을 인수에 setUserInfo를 호출합니다.<br>
setUserInfo에 입력한 유저 정보는 trackEvent가 실행되는 타이밍에 송신합니다.

```cs
using Cyz;
...

string userInfo = "{'guid':'xxxxxxxxxxx', 'ext':{'XXXX':'XXXXX', 'XXXXX':'XXXXXXX'}}";
Fox.setUserInfo(userInfo);
```

## 연계 미디어별 상세
* [DynalystGames](dynalyst_games/README.md)


---
[돌아가기](../README.md)

[톱](../../../README.md)
