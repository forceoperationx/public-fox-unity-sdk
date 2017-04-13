[TOP](../../../README.md)　>　[Unity 플러그인의 도입 절차](../README.md)　>　**iOS 프로젝트의 상세 설정**

---

# iOS 프로젝트의 상세 설정

## **Xcode 프로젝트의 설정**

### Xcode 프로젝트의 퍼블리쉬

iOS용의 프로젝트를 작성하기 위하여 다음의 절차로 Xcode프로젝트를 퍼블리쉬하여 Xcode상에 필요한 설정을 합니다.

1. 메뉴의 「File」>「BUild Settings…」를 선택합니다.
2. Platform의 「iOS」를 선택하여「Switch Platform」을 클릭합니다.
3. 「Player Settings」를 클릭하여 Inspector에 자신의 환경에 맟추어 설정을 합니다.
4. 	「Build」또는 「Build And Run」를 클릭하여 Xcode프로젝트의 퍼블리쉬를 합니다.

### Xcode 프로젝트의 편집

퍼블리쉬한 Xcode프로젝트를 열어 편집합니다.

* **프레임워크 설정**

다음의 프레임워크를 프로젝트에 링크하여 주십시오

<table>
<tr><th>프레임워크명</th><th>Status</th></tr>
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>Security.framework</td><td>Required </td></tr>
<tr><td>SystemConfig.framework</td><td>Required </td></tr>
<tr><td>WebKit.framework</td><td>Required </td></tr>
</table>

* **App Transport Security에 대하여**

F.O.X SDK ver4.0.0에서 부터는 계측을 위한 모든 통신을 HTTPS를 이용하고 있기때문에 추가로 대응하실 필요는 없습니다.


---
[돌아가기](../README.md)

[톱](../../../README.md)
