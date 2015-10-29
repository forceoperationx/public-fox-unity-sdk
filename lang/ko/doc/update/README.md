## 최신 버전으로 업데이트에 대하여

이전 버전의 F.O.X SDK가 도입된 앱에 대한 최신 버전의 F.O.X SDK를 도입하는데 필요한 단계를 설명합니다.

1. 이전 버전의 파일이 프로젝트에 포함되어 있으면 그것을 모두 삭제합니다
2. 최신 버전의 파일을 프로젝트에 추가합니다
3. 「Plugins/FoxPlugin」 및 「Plugins/FoxAnalyticsSession」을 MainCamera 등에 드래그 앤 드롭으로 추가함으로써 구현할 경우 이전 버전의 파일을 모두 지운 다음 다시 새로운 파일을 드래그 & 드롭으로 추가하십시오.
4. iOS의 경우는 Unity 빌드 후 일어서 Xcode에서 AppAdForce.plist을 다시 작성하십시오.


v2.14부터 Android는 광고 ID에 대응하고 있습니다.
광고 ID를 취득하는 경우는 「[광고 ID를 이용하기위한 Google Play Services SDK의 도입](/lang/ko/doc/google_play_services/ja/README.md)」를 확인하십시오.

또한 v2.14부터 Engagement측정 기능이 추가되어 있습니다.
Engagement측정을 실시하는 경우, iOS는 FoxReengagePlugin.m/h를 프로젝트에 추가하십시오. Android 앱의 경우는 「Engagement 측정 구현」를 확인 후 구현합니다.

SDK의 업데이트후에는 반드시 효과 측정 테스트를 실시하고 계측 및 앱의 작동에 문제가 없음을 확인하십시오.
또 「Engagement 측정을 실시하는 경우는 Engagement측정용 테스트를 실시해야합니다.

---
[TOP](/lang/ko/doc/README.md)
