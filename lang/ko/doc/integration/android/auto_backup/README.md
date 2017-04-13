[TOP](../../../../README.md)　>　[Unity 플러그인의 도입 절차](../../README.md)　>　[Android 프로젝트의 상세 설정](../README.md)　>　**오토 백업을 이용한 중복 배제 설정**

---

# 오토 백업을 이용한 중복 배제 기능의 향상

Android M(6.0)부터 추가된 오토 백업을 이용하여 중복 배제 기능을 향상시킬 수 있습니다.
이 대응은 임의 사항입니다.

## 이용 가능 조건

* Android M 단말에서 Android M이상의 단말에 백업 데이터를 복원한 경우입니다.
* 백업시과 복원시에 단말 로그인한 Google계정이 동일할 필요가 있습니다.
* 데이터를 옮기기 위해서는 다음 그림과 같이 단말의 설정이 유저에 의해 유효로 되어있는 것이 필수입니다.

![설정 화면](./img01.png)

## 설정
톱
　이용시에는 애플리케이션 백업 설정 파일의 상화에 맞추어 설정 하실 필요가 있습니다.

> [참고 설정](https://developer.android.com/training/backup/autosyncapi.html)

**[백업하는 애플리케이션 데이터를 개별로 지정하는 경우]**

　이하의 파일이 백업 대상에 포함되도록 할 필요가 있습니다.

```
<include domain="file" path="__ADMAGE_RANDOM_DEVICE_ID__" />
```

**[모든 애플리케이션 백업을 하실 경우]**

　애플리케이션 데이터 전부를 백업 대상이 되도록 설정하신 경우 이하의 파일은 제외되도록 설정하여 주십시오.

```
<exclude domain="file" path="__ADMAGE_WEB_CONVERSION_COMPLETED__" />
<exclude domain="file" path="__ADMAGE_APP_CONVERSION_COMPLETED__" />
```

---
[돌아가기](../README.md)

[톱](../../../../README.md)
