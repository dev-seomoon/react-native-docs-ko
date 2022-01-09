# 개발 환경 설정

이 페이지는 첫 번째 React Native 앱을 설치하고 빌드하는 데 도움이 됩니다. 

***\*모바일 개발이 처음인 경우\****, 가장 쉽게 시작하는 방법은 Expo CLI를 사용하는 것입니다. Expo는 React Native를 중심으로 구축된 툴들의 모음이며, 많은 [기능들](features)을 가지고 있지만 지금 우리에게 가장 필요한 기능은 몇 분 안에 React Native 앱을 작성할 수 있게 해주는 기능입니다. 최신 버전의 Node.js와 휴대폰 또는 에뮬레이터만 있으면 됩니다. 툴을 설치하기 전에 웹 브라우저에서 먼저 React Native를 사용해보고 싶다면, [Snack](https://snack.expo.io/)을 사용해보세요. 

***\*이미 모바일 개발 경험이 있다면\****, React Native CLI를 사용해도 좋습니다. React Native CLI를 시작하려면 XCode 또는 Android Studio가 필요합니다. 이미 이러한 툴이 설치되어 있다면, 몇 분 내에 바로 실행할 수 있습니다. 설치되어 있지 않다면, 설치 및 설정에 한 시간 정도가 소요됩니다. 

## Expo CLI Quickstart

Node 12 LTS 이상 버전이 설치되어 있다고 가정하고, npm을 사용해 Expo CLI 커맨드 라인 유틸리티를 설치할 수 있습니다. 

- npm 

  ```shell
  npm install -g expo-cli
  ```

- yarn

  ```shell
  yarn global add expo-cli
  ```

그리고 "AwesomeProject"라는 이름의 새로운 React Native 프로젝트를 생성하기 위해 다음 명령을 실행합니다. 

- npm

  ```shell
  expo init AwesomeProject
  
  cd AwesomeProject
  npm start # you can also use: expo start
  ```

- yarn

  ```shell
  expo init AwesomeProject
  
  cd AwesomeProject
  yarn start # you can also use: expo start
  ```

 위 명령을 실행하면 개발 서버가 시작됩니다. 

## React Native 애플리케이션 실행하기

Expo 클라이언트 앱을 iOS나 Android 휴대폰에 설치하고, 컴퓨터와 동일한 무선 네트워크에 연결합니다. Android의 경우, 터미널에서 프로젝트를 열고 Expo 앱으로 QR 코드를 스캔합니다. iOS의 경우, 카메라 앱의 내장 QR 코드 스캐너를 사용합니다. 

### 앱 수정하기

이제 성공적으로 앱을 실행했으므로, 수정을 해봅시다. 원하는 텍스트 에디터에서 `App.js`를 열고 몇 줄을 수정해보세요. 변경사항을 저장하면 애플리케이션이 자동으로 리로드됩니다. 

### 끝!

축하합니다! 성공적으로 첫 번째 React Native 앱을 실행하고 수정했습니다. 

![img](https://reactnative.dev/docs/assets/GettingStartedCongratulations.png)

### 이제 무엇을 하나요?

Expo 툴과 관련된 질문이 있는 경우, Expo에 참조할 수 있는 [문서](https://docs.expo.io/)가 있습니다. [Expo 포럼](https://forums.expo.io/)에서 도움을 요청할 수도 있습니다. 

Expo와 같은 도구를 사용하면 빠르게 시작할 수 있지만, Expo CLI를 사용해 앱을 구축하기 전에는 [제한 사항](https://docs.expo.io/versions/latest/introduction/why-not-expo/)에 대해 먼저 알아보는 것이 좋습니다. 

Expo와 관련해 문제가 있다면, 새로운 issue를 생성하기 전에 관련된 이슈가 존재하는지 먼저 확인해주세요. 

- [Expo CLI issues](https://github.com/expo/expo-cli/issues) (Expo CLI 관련 이슈)

-  [Expo issues](https://github.com/expo/expo/issues) (Expo 클라이언트 또는 SDK 관련 이슈)

React Native에 대해 더 알고 싶다면, [React Native에 대한 소개](https://reactnative.dev/docs/getting-started)를 확인하세요. 

### 시뮬레이터나 가상 디바이스에서 앱 실행하기

Expo CLI를 사용하면 React Native 앱을 실제 디바이스에서 개발 환경 설정 없이 실행할 수 있습니다. iOS 시뮬레이터나 Android 가상 디바이스에서 앱을 실행하고 싶다면, "React Native CLI Quickstart"의 지침을 참고해 XCode 설치하는 방법 또는 Android 개발 환경 설정하는 방법을 알아보세요. 

설정이 끝나면, Android 가상 디바이스에서 `npm run android`를 실행해 앱을 시작하거나, iOS 시뮬레이터에서 `npm run ios` (macOS only) 를 실행해 앱을 시작할 수 있습니다. 

### 주의사항

Expo를 사용해 프로젝트를 만들 때 네이티브 코드를 빌드하지 않으므로, Expo 클라이언트 앱에서 사용할 수 있는 React Native API 및 컴포넌트 이외의 사용자 지정 네이티브 모듈은 포함할 수 없습니다. 

결국 네이티브 코드를 include 해야 한다는 점을 고려하더라도, Expo는 좋은 시작이 될 수 있습니다. 이 경우 직접 네이티브 빌드를 생성하기 위해서는 "eject"를 해야 합니다. eject를 실행한다면, 프로젝트를 계속 진행하기 위해 "React Native CLI Quickstart" 지침을 읽어봐야 합니다. 

Expo CLI는 프로젝트가 Expo 클라이언트 앱에서 지원되는 최신 React Native 버전을 사용하도록 설정합니다. Expo 클라이언트 앱은 일반적으로 React Native 버전이 stable로 릴리즈되고 나서 약 일주일 후에 해당 React Native 버전을 지원하게 됩니다. 어떤 버전이 지원되는지 알아보려면 [이 문서](https://docs.expo.io/versions/latest/sdk/overview/#sdk-version)를 확인해보세요. 

React Native를 이미 존재하는 프로젝트에 통합하는 중이라면, Expo CLI를 건너뛰고 바로 네이티브 빌드 환경 설정으로 가세요. 위에서 "React Native CLI Quickstart"를 선택해 React Native 네이티브 빌드 환경 설정에 대한 가이드를 볼 수 있습니다. 