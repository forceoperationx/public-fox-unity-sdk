[TOP](../../README.md)　>　**인스톨 계측의 상세**

---

# 인스톨 계측의 상세

`trackInstall`메소드를 이용하여 인스톨 계측을 할 수 있습니다.<br>
기동시에 실행되는 스크립트에서 이하의 코드가 불려지도록 실장하여 주십시오<br>
특정의 URL에 리다이렉트 하시는 경우 및 애플리케이션에서 동적인 URL을 생성하시는 경우 URL의 문자열을 설정하여 주십시오

```cs
using Cyz;
...

  FoxTrackOption option = new FoxTrackOption();
  // URL을 지정
  option.redirectURL = "http://yourhost.com/yourpage.html";
  Fox.trackInstall(option);
```

### Buid (광고주 단말 ID)를 지정합니다.

`FoxTrackOption`의 buid에 광고주 단말ID를 넘길수 있습니다.<br>
예를들어 애플리케이션 기동시에 SDK가 UUID를 생성하여 초기기동의 성과와 연관지어 관리하는 경우등에 이용 가능합니다.

```cs
using Cyz;
...

  FoxTrackOption option = new FoxTrackOption();
  option.redirectURL = "http://yourhost.com/yourpage.html";
  // BUIDを指定
  option.buid = "USER_001"
  Fox.trackInstall(option);
```

<div id="check_track"></div>

### 인스톨 계측이 완료되었는가를 체크합니다.

초기 기동시의 Fox.trackInstall가 실행완료 되었는지에 대한 정보를 bool로 취득하는 것이 가능합니다.<br>
이하 메소드를 이용하여 인스톨 계측이 완료되어 ２회 이후의 기동시에 특정의 처리를 행하는 것이 가능하게 되었습니다.

```cs
using Cyz;
...
  // 계측 완료인지 확인
  bool isComplete = Fox.isConversionCompleted();
```


<div id="receive_callback"></div>

### 콜백을 수령

`FoxTrackOption`의 `onTrackComplete`（이벤트 핸들러）를 이용하여 F.O.X SDK의 계측 처리가 완료된 타이밍에 콜백을 받는것이 가능합니다.

```cs
using Cyz;
...

  FoxTrackOption option = new FoxTrackOption();
  option.redirectURL = "http://yourhost.com/yourpage.html";
  option.buid = "USER_001"
  // 이벤트 핸들러를 지정
  option.onTrackComplete += HandleFoxTrackComplete;
  Fox.trackInstall(option);
...

// SDK의 기동 계측이 왼료된 타이밍에 호출됩니다.
public void HandleFoxTrackComplete(object sender, EventArgs args) {
		print ("FoxTrack tracked complete!!");
}
```

> ※ 버전4.1.1이후에 있어 Android에 콜백을 받는 경우 `UnityPlayerActivity`를 편집할 필요가 있으므로 [이곳](../integration/android/README.md#receive_callback)을 확인하여 주십시오

### opt out의 설정

광고 회사에 따라 타겟팅 광고에 이용되지 않는 것을 유저가 선택하는 것이 가능합니다.<br>애플리케이션의 기동시에 있어 프라이버시 폴리시 및 이용 규약을 표시하는 다이얼로그에 유저가 opt out을 선택한 경우 효과 측정의 결과의 통지와 더불어 F.O.X가 광고 회사에 대하여 그 유저가 opt out를 선택한 것을 통지합니다.

opt out에 대응하는 경우에는 이하와 같이 「`Fox.trackInstall`의 인수에 설정을 하여 주십시오

```cs
using Cyz;
...

  // 유저가 opt out을 선택하는 경우에는 setOptout를 유효로 합니다.
  FoxTrackOption option = new FoxTrackOption();
  option.redirectURL = "https://www.yourhost.com";
  option.buid = "USER_001";
  if(user.optout) {
	   option.optout = true;
  }
  Fox.trackInstall(option);
```

> ※ opt out는 default로 false입니다.

---
[TOP](../../README.md)
