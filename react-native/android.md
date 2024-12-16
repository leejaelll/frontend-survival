---
description: Google Play Console - Create production release
cover: >-
  https://images.unsplash.com/photo-1714756123393-929709970fdb?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MTc1NTgwNTF8&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Android 빌드하기

{% hint style="warning" %}
**빌드 전에 AndroidStudio, Python이 설치되어있는지 확인하기**
{% endhint %}

## **version 높이기**

**가장 먼저 해야할 일은 `app.config.js` 파일에서 version, buildNumber, versionCode를 높이는 것.**\
&#xNAN;_**(동일한 버전으로는 심사를 받을 수 없다.)**_

```javascript
version: "24.23.0",
buildNumber: "58",
versionCode: 58,
```

* **version은 연도, 주차, 횟수 순으로 작성한다. 👉🏻 "24.23.0"이라면 24년도 23주차 0번째**
* **buildNumber와 versionCode는 1씩 증가시킨다.**&#x20;

## **Shell Script 파일 실행시키기**

```
./ready_android.sh
```

* **명령어를 입력하고나면 android folder가 생성된다.**&#x20;

## **Android Studio에서 빌드하기**

**file -> open project**

<div align="left"><figure><img src="../.gitbook/assets/스크린샷 2024-06-05 오후 12.49.22.png" alt="" width="375"><figcaption></figcaption></figure></div>

<figure><img src="../.gitbook/assets/스크린샷 2024-06-05 오후 12.53.11.png" alt=""><figcaption></figcaption></figure>

* **Build 탭에서 자동으로 빌드 시작**

{% hint style="danger" %}
_**ERROR**_

**Null extracted folder for artifact: ResolvedArtifact(componentIdentifier=com.facebook.react:hermes-android:0.72.4, variant=com.facebook.react:hermes-android:0.72.4 variant debugVariantDefaultRuntimePublication,**

\
**👉🏻 해당 에러가 발생할 경우,** &#x20;

* **SDK 버전 확인 (Tools - SDK Manager에서 SDK Platforms, SDK Tools 확인)**

&#x20;![](<../.gitbook/assets/스크린샷 2024-06-05 오후 12.58.33.png>)

* **버전이 일치한다면, 오른쪽 상단 'Sync Project with Gradle Files' 클릭**

<img src="../.gitbook/assets/스크린샷 2024-06-05 오후 12.56.01.png" alt="" data-size="original">
{% endhint %}

* **빌드가 정상적으로 완료되었을 경우, 폴더 디렉토리가 다른 형태로 바뀐다.**&#x20;



#### **Build -> Generate Signed App Bundle/APK**&#x20;

<figure><img src="../.gitbook/assets/스크린샷 2024-06-05 오후 1.01.42.png" alt=""><figcaption></figcaption></figure>

<div align="left"><figure><img src="../.gitbook/assets/스크린샷 2024-06-05 오후 12.10.51.png" alt="" width="375"><figcaption></figcaption></figure></div>

* **Android App Bundle: Google Play에 실사용 애플리케이션을 올려야할 때 사용**
* **APK: 테스트용으로 빌드할 때 사용**

<div align="left"><figure><img src="../.gitbook/assets/스크린샷 2024-06-05 오후 1.05.38.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../.gitbook/assets/스크린샷 2024-06-05 오전 11.55.26.png" alt="" width="375"><figcaption></figcaption></figure></div>

* **release create 👉🏻 성공적으로 빌드가 되었다면 release 폴더가 생성된다. `(app-release.apk)`**

## **안드로이드 핸드폰에 테스트 애플리케이션 설치**

```javascript
adb devices
```

{% hint style="danger" %}
_**ERROR**_

#### **adb command not found**

\
**👉🏻 adb 명령어를 사용할 수 있도록 안드로이드 SDK 경로 설정이 필요.**
{% endhint %}

```sh
vi ~/.zshrc

# 터미널 경로 입력
export PATH="$PATH:/Users/bienpr/Library/Android/sdk/platform-tools"

# .zshrc 실행
source ~/.zshrc
```

<figure><img src="../.gitbook/assets/240530-9 (1).png" alt=""><figcaption></figcaption></figure>

```sh
adb install app-relaeas.apk
```

## **google play console release 파일 업로드**

* closed testing ➡️ create new release
*   app-release.aab 업로드

    <figure><img src="../.gitbook/assets/스크린샷 2024-06-05 오후 5.07.15.png" alt=""><figcaption></figcaption></figure>

