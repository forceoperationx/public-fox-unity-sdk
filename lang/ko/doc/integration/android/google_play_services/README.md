[TOP](../../../../README.md)　>　[Unity 플러그인의 도입 절차](../../README.md)　>　[Android 프로젝트의 상세 설정](../README.md)　>　**Google Play Services의 도입**

---

# Google Play Services의 도입

Google Play Services를 프로젝트에 도입합니다.<br>

애플리케이션의 모듈 디렉토리에 있는 build.gradle을 열어 아래와 같이 최신의 Google Play services에의 dependencies의 설정을 추가합니다.


```
dependencies {
	compile 'com.google.android.gms:play-services:9.4.0'
}
```
> ※ GooglePlayServices의 도입 상세는 [`Setting Up Google Play Services | Android Developers`](https://developer.android.com/google/play-services/setup.html)를 확인하여 주십시오


> ※ ADT의 Eclipse서포트 종료에 따라 Eclipse에서의 도입은 폐지하였습니다.<br>개발 환경이 Eclipse인 경우[Google Play Services Jar Resolver Library for Unity](https://github.com/googlesamples/unity-jar-resolver)를 이용하여 GooglePlayServices를 도입하여 주십시오.

## 메소드 수 상한이 64K의 사양에 대해서

Android앱에서는 보유할수 있는 메소드 수에 64K(65536)의 상한이 있습니다. 초과할경우 빌드 에러가 발행합니다.<br>
그로 인해 많은 메소드를 보유하고 있는 GooglePlayServices를 광고 식별ID 취득만을 위하여 도입하실 경우에는<br>
광고 식별ID의 취득을 목적으로하는 아래의 라이브러리를 사용하여 주십시오.<br>
F.O.X SDK에서는 GooglePlayServices를 광고 식별ID의 취득을 하기 위하여 이용하고 있습니다.

```
dependencies {
	compile 'com.google.android.gms:play-services-ads:9.4.0'
}
```

> ※ 메소드 수 상한 64K의 사양에 대한 상세<br>
[Configure Apps with Over 64K Methods | Android Developers](https://developer.android.com/studio/build/multidex.html)

> ※ Google Play services의 최신 버전은 Android의 개발자 사이트에서 확인하여 주십시오.<br>
[Google Play Services | Android Developers](https://developer.android.com/google/play-services/index.html)


## Google Play 개발자 프로그램 준수

Force Operation X And​​roid SDK는 Google Play 개발자 프로그램 정책을 준수하고 있습니다. 이 SDK는 정책을 준수하기 위해 영구적인 디바이스 ID (IMEI, MAC어드레스 및 AndroidID)를  취득하는 경우에는 광고ID가 취득 되지 않습니다. 2014년 8월 1일부터 Google Play 스토어에 업로드 된 모든 업데이트와 새로운 앱은 광고목적으로 사용하는 단말ID는 광고ID를 이용할 필요가 있습니다.본 정책을 준수하기 위해 아래의 단계를 수행하십시오.

---
[トップ](../../README.md)
