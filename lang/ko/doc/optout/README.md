## (옵션) 탈퇴 구현

광고 회사에 의해 타겟팅 광고에 이용되지 않는 것을 사용자에게 선택하도록 할 수 있습니다. 앱 시작시에 개인 정보 보호 정책 및 이용 약관을 표시하는 대화 상자에서 사용자가 탈퇴를 선택한 경우 효과 측정 결과의 통지와 함께, F.O.X가 광고 회사에 대하여 사용자가 탈퇴를 선택한 것을 통지합니다.
탈퇴에 해당하는 경우는 다음과 같습니다 "설치 계측 구현"에서 구현한 FoxPlugin.sendConversion 이전에 설정을 해주십시오.

```C#
// 사용자가 탈퇴를 선택한 경우 setOptout를 활성화
if(user.optout) {
	FoxPlugin.setOptout(true);
}
FoxPlugin.sendConversion("default");
```