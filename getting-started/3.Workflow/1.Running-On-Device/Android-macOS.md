# 디바이스에서 실행하기

앱을 릴리즈하기 전에 실제 기기에서 테스트해보는 것이 좋습니다. 이 문서는 디바이스에서 React Native 앱을 실행하고 배포 준비를 하기 위해서 필요한 단계들을 안내합니다. 

Expo CLI나 Create React Native App을 사용해서 프로젝트를 설정한 경우, Expo 앱에서 QR 코드를 스캔해 디바이스에서 앱을 미리보기할 수 있습니다. 그러나 디바이스에서 앱을 빌드하고 실행하기 위해서는, [환경 설정 가이드](https://reactnative.dev/docs/environment-setup) 에서 네이티브 코드 종속성을 eject하고 설치해야합니다. 

## Android 기기에서 앱 실행하기

#### 개발 OS: macOS

### 1. USB를 통한 디버깅 활성화하기

대부분의 Android 기기는 기본적으로, Google Play에서 다운로드받은 앱만 설치하고 실행할 수 있습니다. 개발 단계에 기기에 앱을 설치하려면 기기에서 USB 디버깅을 활성화해야 합니다. 

USB 디버깅을 활성화하려면, 먼저 **설정** -> **휴대폰 정보** -> **소프트웨어 정보**에서 "개발자 옵션" 메뉴를 활성화해야 합니다. 그리고 나서 아래 쪽의 `빌드 번호` 를 일곱 번 터치합니다. 그리고 **설정** -> **개발자 옵션**으로 돌아가서 "USB 디버깅"을 활성화할 수 있습니다. 

### 2. USB를 통해 기기 연결하기

이제 React Native 프로젝트를 실행할 Android 기기를 설정합니다. USB를 통해 개발 컴퓨터에 디바이스를 연결합니다. 

이제 `adb devices` 를 실행해 기기가 ADB (Android Debug Bridge) 에 올바르게 연결되었는지 확인하십시오. 

```shell
$ adb devices
List of devices attached
emulator-5554 offline   # Google emulator
14ed2fcc device         # Physical device
```

오른쪽 열에 `device` 가 보이면, 기기가 연결된 것입니다. **한 번에 하나의 기기만 연결**해야 합니다.

### 3. 앱 실행하기

명령 프롬프트에 다음을 입력하여 기기에 앱을 설치하고 시작합니다. 

```shell
$ npx react-native run-android
```

> "브릿지 설정을 사용할 수 없습니다" 오류 발생 시 [adb reverse 사용](https://reactnative.dev/docs/running-on-device#method-1-using-adb-reverse-recommended)을 참고하세요. 

> 힌트: `React Native CLI` 를 사용해서 `Release` 빌드를 생성하고 실행할 수도 있습니다. (예: `npx react-native run-android --variant=release`).

## 개발 서버에 접속하기

개발 컴퓨터에서 실행중인 개발 서버에 접속해 기기에서 빠르게 반복할 수도 있습니다. USB 케이블 또는 Wi-Fi 네트워크에 액세스할 수 있는지에 따라, 여러 가지 방법으로 이 작업을 수행할 수 있습니다. 

### 방법 1: adb reverse 사용하기 (권장)

Android 5.0 (Lollipop) 이상 기기에서 USB 디버깅이 활성화되어 있고, 개발 컴퓨터에 USB를 통해 연결되어 있는 경우 이 방법을 사용할 수 있습니다. 

명령 프롬프트에서 다음을 실행하세요. 

```shell
$ adb -s <device name> reverse tcp:8081 tcp:8081
```

장치 이름을 찾으려면 다음 adb 명령을 실행하세요. 

```shell
$ adb devices
```

이제 [개발자 메뉴](https://reactnative.dev/docs/debugging#accessing-the-in-app-developer-menu)에서 라이브 리로딩을 활성화할 수 있습니다. 앱은 JavaScript 코드가 변경될 때마다 다시 로드됩니다. 

### 방법 2: Wi-Fi를 통해 연결하기

Wi-Fi를 통해 개발 서버에 연결할 수도 있습니다. 먼저 USB 케이블을 사용해 기기에 앱 설치가 완료되면, 다음 지침에 따라 무선으로 디버깅할 수 있습니다. 진행하기 전에 개발 컴퓨터의 현재 IP 주소가 필요합니다. 

**시스템 설정** -> **네트워크** 에서 해당 IP 주소를 찾을 수 있습니다. 

1. 컴퓨터(노트북)과 휴대폰이 **동일한** Wi-Fi 네트워크에 연결되어 있는지 확인하세요. 
2. 디바이스에서 React Native 앱을 실행하세요. 
3. 그러면 [오류와 함께 붉은색 화면](https://reactnative.dev/docs/debugging#in-app-errors-and-warnings)이 뜨지만, 다음 단계에서 해결할 수 있습니다. 
4. 인앱 [개발자 메뉴](https://reactnative.dev/docs/debugging#accessing-the-in-app-developer-menu)에 들어가세요.
5. **개발 설정** → **장치 디버깅 서버 호스트 & 포트**로 이동하세요. 
6. 컴퓨터(노트북)의 IP 주소와 로컬 개발 서버의 포트를 입력하세요. (예: 10.0.1.1:8081).
7. **개발자 메뉴**로 돌아가서 **JS 리로드**를 선택하세요.

이제 [개발자 메뉴](https://reactnative.dev/docs/debugging#accessing-the-in-app-developer-menu)에서 라이브 리로딩을 활성화할 수 있습니다. 앱은 JavaScript 코드가 변경될 때마다 다시 로드됩니다. 

## 배포를 위해 앱 빌드하기

React Native를 사용해 멋진 앱을 만들었습니다. 이제 Play Store에서 이 앱을 릴리즈해봅시다. 이 과정은 다른 모든 네이티브 Android 앱과 동일하며, 몇 가지 추가적인 고려사항이 있습니다. 자세한 내용은 [signed APK 생성하기](https://reactnative.dev/docs/signed-apk-android)를 참고하십시오. 