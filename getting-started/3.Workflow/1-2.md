<-- iOS/macOS -->

# 디바이스에서 실행하기

앱을 릴리즈하기 전에 실제 기기에서 테스트해보는 것이 좋습니다. 이 문서는 디바이스에서 React Native 앱을 실행하고 배포 준비를 하기 위해서 필요한 단계들을 안내합니다. 

Expo CLI나 Create React Native App을 사용해서 프로젝트를 설정한 경우, Expo 앱에서 QR 코드를 스캔해 디바이스에서 앱을 미리보기할 수 있습니다. 그러나 디바이스에서 앱을 빌드하고 실행하기 위해서는, [환경 설정 가이드](https://reactnative.dev/docs/environment-setup) 에서 네이티브 코드 종속성을 eject하고 설치해야합니다. 

## iOS 기기에서 앱 실행하기

#### 개발 OS: macOS

> iOS 기기용 앱을 구축하려면 Mac이 필요합니다. 대신에 Expo CLI를 사용하여 앱을 만드는 방법에 대해 알아보려면 [환경 설정 가이드](https://reactnative.dev/docs/environment-setup)를 참고하십시오. Expo 클라이언트 앱을 사용해 앱을 실행할 수 있습니다. 

### 1. USB를 통해 장치 연결하기

USB 라이트닝 케이블을 사용해 Mac에 iOS 기기를 연결하세요. 프로젝트의 `ios` 폴더로 들어가서 `.xcodeproj` 파일을 열거나, CoCoaPods을 사용하는 경우 XCode를 사용해 해당 폴더에서 `.xcworkspace` 파일을 엽니다. 

iOS 기기에서 앱을 처음으로 앱을 실행하는 경우, 기기를 개발용으로 등록해야할 수도 있습니다. XCode 메뉴 바에서 **Production** 메뉴를 연 다음 **Destination**으로 이동합니다. 목록에서 기기를 찾아 선택합니다. 그러면 XCode가 기기를 개발용으로 등록합니다. 

### 2. Configure code signing

아직 [Apple 개발자 계정]()이 없다면 계정을 등록하십시오. 

XCode Project Navigator에서 프로젝트를 선택한 다음 메인 타겟을 선택합니다. (프로젝트와 동일한 이름을 가져야 함). "일반" 탭을 찾습니다. "Signing"으로 이동하여 Team 드롭다운 하위에서 Apple 개발자 계정 또는 팀이 선택되었는지 확인합니다. 테스트 타겟에 대해서도 동일한 작업을 수행하세요. (메인 타겟 아래에 있으며, "테스트"로 끝남). 

프로젝트의 **테스트** 타겟에 대해 이 단계를 **반복하세요. **

![img](https://reactnative.dev/assets/images/RunningOnDeviceCodeSigning-daffe4c45a59c3f5031b35f6b24def1d.png)

### 3. 앱 빌드하고 실행하기

모든 것이 올바르게 설정되었다면, 장치가 XCode 툴바의 빌드 대상으로 나열되고, 디바이스 창 (`⇧⌘2`)에도 나타납니다. 이제 **빌드 후 실행** 버튼을 누르거나 (`⌘R`) **Product** 메뉴에서 **실행**을 선택합니다. 앱이 기기에서 곧 시작됩니다. 

![img](data:image/png;base64,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)

> 문제가 발생하면 Apple의 [기기에서 앱 시작하기](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/LaunchingYourApponDevices/LaunchingYourApponDevices.html#//apple_ref/doc/uid/TP40012582-CH27-SW4) 문서를 살펴보세요. 

## 개발 서버에 접속하기

개발 컴퓨터에서 실행중인 개발 서버에 접속해 기기에서 빠르게 반복할 수도 있습니다. USB 케이블 또는 Wi-Fi 네트워크에 액세스할 수 있는지에 따라, 여러 가지 방법으로 이 작업을 수행할 수 있습니다. 기기를 흔들어서 [개발자 메뉴](https://reactnative.dev/docs/debugging#accessing-the-in-app-developer-menu)를 열고, 라이브 리로드를 활성화하세요. 이제 앱은 JavaScript 코드가 변경될 때마다 다시 로드됩니다. 

![img](https://reactnative.dev/assets/images/DeveloperMenu-f22b01f374248b3242dfb3a1017f98a8.png)

### 문제 해결

> 문제가 있는 경우, Mac과 디바이스가 동일한 네트워크에 있고 서로 접근할 수 있는지 확인하십시오. 종속 포털이 있는 많은 개방형 무선 네트워크는 기기가 네트워크 상의 다른 기기에 접근하지 못하도록 설정되어 있습니다. 이 경우 기기의 개인 핫스팟 기능을 사용해야할 수도 있습니다. 또한 매우 빠른 전송 속도를 위해 Mac에서 기기로 USB를 통해 인터넷(Wifi/이더넷) 연결을 공유하고 이 터널을 통해 번들러에 연결할 수도 있습니다. 

개발 서버에 연결하려고 할 때 [오류가 있는 붉은색 화면](https://reactnative.dev/docs/debugging#in-app-errors-and-warnings)이 다음 설명과 함께 표시될 수 있습니다. 

> `http://localhost:8081/debugger-proxy?role=client`에 대한 연결 시간이 초과되었습니다. 노드 프록시를 실행 중입니까? 장치에서 실행 중인 경우, `RCTWebSocketExecutor.m`에 정확한 IP 주소가 있는지 확인하십시오.

이 문제를 해결하려면 다음 사항을 확인하십시오. 

#### 1. Wi-Fi 네트워크.

컴퓨터(노트북)과 휴대폰이 **동일한 ** Wi-Fi 네트워크에 연결되어 있는지 확인하세요. 

#### 2. IP 주소

빌드 스크립트가 컴퓨터의 IP 주소를 정확하게 인식했는지 확인하세요. (예: 10.0.1.123)

![img](https://reactnative.dev/assets/images/XcodeBuildIP-dfc8243436f5436466109acb8f9e0502.png)

**Report navigator** 탭을 열고, 최근 **빌드**를 선택한 후  `IP=` 다음에 IP 주소를 검색합니다. 앱에 포함된 IP 주소는 컴퓨터의 IP 주소와 일치해야 합니다. 

## 배포를 위해 앱 빌드하기

React Native를 사용해 멋진 앱을 만들었습니다. 이제 App Store에서 이 앱을 릴리즈해봅시다. 이 과정은 다른 모든 네이티브 iOS 앱과 동일하며, 몇 가지 추가적인 고려사항이 있습니다.

Follow the guide for [publishing to the Apple App Store](https://reactnative.dev/docs/publishing-to-app-store) to learn more.