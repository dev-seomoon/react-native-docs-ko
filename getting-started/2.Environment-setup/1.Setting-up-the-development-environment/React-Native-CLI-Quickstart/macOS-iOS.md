<-- Tab: React Native CLI Quickstart -->

<-- Inner Tab: macOS/iOS -->

# 개발 환경 설정

이 페이지는 React Native 앱을 설치하고 빌드하는 첫 걸음에 도움이 됩니다.

**모바일 개발이 처음인 경우**, 시작하는 가장 쉬운 방법은 Expo CLI를 사용하는 것입니다. Expo는 React Native를 중심으로 구축 된 도구 모음이며 [features](https://expo.io/features)이 많지만 현재 가장 관련성이 높은 기능은 몇 분 안에 React Native 앱을 작성할 수 있다는 것입니다. 최신 버전의 Node.js와 핸드폰 또는 에뮬레이터만 있으면 됩니다. 도구를 설치하기 전에 웹 브라우저에서 직접 React Native를 사용해보고 싶다면 [Snack](https://snack.expo.io/)를 사용해보세요.

**이미 모바일 개발에 익숙한 경우**, React Native CLI를 사용할 수 있습니다. 시작하려면 Xcode 또는 Android Studio가 필요합니다. 이러한 도구 중 하나가 이미 설치되어있는 경우 몇 분 내에 시작하여 실행할 수 있습니다. 설치되지 않은 경우 설치 및 구성하는 데 약 1시간이 소요됩니다.

## React Native CLI 빠르게 시작하기

프로젝트에서 네이티브 코드를 빌드해야하는 경우 다음 지침을 따르세요. 예를 들어 React Native를 기존 애플리케이션에 통합하거나 Expo에서 "ejected"한 경우 이 섹션이 필요합니다.

지침은 개발 운영 체제 및 iOS 또는 Android 용 개발을 시작할지 여부에 따라 약간 다릅니다. Android와 iOS 공용으로 개발하고 싶다면 괜찮습니다. 설정이 약간 다르기 때문에 어떤 것을 시작할지 하나를 선택할 수 있습니다.

#### 개발 OS: macOS

#### 타겟 OS: iOS

## 종속성 설치하기

Node, Watchman, React Native command line 인터페이스, Xcode 및 CocoaPods가 필요합니다.

선택한 편집기를 사용하여 앱을 개발할 수 있지만 iOS 용 React Native 앱을 빌드하는 데 필요한 도구를 설정하려면 Xcode를 설치해야합니다.

### Node 및 Watchman

[Homebrew](http://brew.sh/)를 사용하여 Node 및 Watchman을 설치하는 것이 좋습니다. Homebrew를 설치 한 후 터미널에서 다음 명령을 실행하십시오:

```shell
brew install node
brew install watchman
```

시스템에 Node를 이미 설치 한 경우 Node 12 이상인지 확인하십시오.

[Watchman](https://facebook.github.io/watchman)은 파일 시스템의 변경 사항을 감시하기위한 Facebook 도구입니다. 더 나은 성능을 위해 설치하는 것이 좋습니다.

### Xcode

Xcode를 설치하는 가장 쉬운 방법은 [Mac App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)를 사용하는 것입니다. Xcode를 설치하면 iOS 시뮬레이터와 iOS 앱을 빌드하는 데 필요한 모든 도구도 설치됩니다.

시스템에 Xcode를 이미 설치 한 경우 버전 10 이상인지 확인하십시오.

#### 명령 행 도구

Xcode Command Line 도구도 설치해야합니다. Xcode를 연 다음 Xcode 메뉴에서 "Preferences..."을 선택합니다. 위치 패널로 이동하고 Command Line 도구 드롭 다운에서 최신 버전을 선택하여 도구를 설치합니다.

![Xcode Command Line Tools](https://reactnative.dev/assets/images/GettingStartedXcodeCommandLineTools-8259be8d3ab8575bec2b71988163c850.png)

#### Xcode에서 iOS Simulator 설치하기

시뮬레이터를 설치하려면 **Xcode > Preferences...** 를 열고 **Components** 탭을 선택합니다. 사용하려는 iOS 버전이있는 시뮬레이터를 선택하십시오.

#### CocoaPods

[CocoaPods](https://cocoapods.org/)는 Ruby로 빌드되었으며 macOS에서 사용할 수있는 기본 Ruby로 설치할 수 있습니다. Ruby 버전 관리자를 사용할 수 있지만 수행중인 작업을 모르는 경우 macOS에서 사용할 수있는 표준 Ruby를 사용하는 것이 좋습니다.

기본 Ruby 설치를 사용하려면 gem을 설치할 때 `sudo`를 사용해야합니다. (하지만 이것은 gem 설치 중에 만 문제가됩니다.)

```shell
sudo gem install cocoapods
```

더 많은 정보는 [CocoaPods Getting Started guide](https://guides.cocoapods.org/using/getting-started.html)를 참고하세요.

### React Native 명령 행 도구

React Native에는 기본 제공 command line 인터페이스가 있습니다. CLI의 특정 버전을 전역적으로 설치하고 관리하는 대신 Node.js와 함께 제공되는 `npx`를 사용하여 런타임에 현재 버전에 액세스하는 것이 좋습니다. `npx react-native <command>`를 사용하면 명령이 실행될 때 현재 안정된 버전의 CLI가 다운로드되고 실행됩니다.

## 새 애플리케이션 생성하기

> 이전에 글로벌 `react-native-cli` 패키지를 설치 한 경우 예기치 않은 문제가 발생할 수 있으므로 제거하십시오.

React Native의 내장 명령 줄 인터페이스를 사용하여 새 프로젝트를 생성 할 수 있습니다. "AwesomeProject" 라는 새로운 React Native 프로젝트를 만들어 보겠습니다.

```shell
npx react-native init AwesomeProject
```

React Native를 기존 애플리케이션에 통합하거나 Expo에서 "ejected"한 경우 또는 기존 React Native 프로젝트에 Android 지원을 추가하는 경우에는 필요하지 않습니다. ([Integration with Existing Apps](https://reactnative.dev/docs/integration-with-existing-apps)를 참고하세요.) [Ignite CLI](https://github.com/infinitered/ignite)와 같은 타사 CLI를 사용하여 React Native 앱을 초기화 할 수도 있습니다.

### [선택] 특정한 버전이나 템플릿 사용하기

특정 React Native 버전으로 새 프로젝트를 시작하려면 `--version` 인수를 사용할 수 있습니다:

```shell
npx react-native init AwesomeProject --version X.XX.X
```

TypeScript와 같은 사용자 지정 React Native 템플릿을 `--template` 인수와 함께 사용하여 프로젝트를 시작할 수도 있습니다.

```shell
npx react-native init AwesomeTSProject --template react-native-template-typescript
```

> **참고** 위 명령이 실패하면 이전 버전의 `react-native` 또는 `react-native-cli`가 PC에 전역적으로 설치되어있을 수 있습니다. cli를 제거하고 `npx`를 사용하여 cli를 실행하십시오.

## React Native 애플리케이션 실행하기

### 1단계: Metro 시작하기

먼저 React Native와 함께 제공되는 JavaScript 번들러 인 Metro를 시작해야합니다. Metro는 "입력 파일과 다양한 옵션을 가져 와서 모든 코드와 해당 종속성을 포함하는 단일 JavaScript 파일을 반환합니다."—[Metro Docs](https://facebook.github.io/metro/docs/concepts)

Metro를 시작하려면 React Native 프로젝트 폴더 내에서`npx react-native start`를 실행하세요:

```shell
npx react-native start
```

`react-native start`은 Metro Bundler를 시작합니다.

> Yarn 패키지 관리자를 사용하는 경우 기존 프로젝트 내에서 React Native 명령을 실행할 때 `npx` 대신`yarn`을 사용할 수 있습니다.

> 웹 개발에 익숙하다면 Metro는 React Native 앱용 webpack과 매우 유사합니다. Kotlin 또는 Java와 달리 JavaScript는 컴파일되지 않으며 React Native도 아닙니다. 번들링은 컴파일과 동일하지 않지만 시작 성능을 향상시키고 일부 플랫폼 별 JavaScript를 더 광범위하게 지원되는 JavaScript로 변환하는 데 도움이 될 수 있습니다.

### 2단계: 애플리케이션 시작하기

Metro Bundler가 자체 터미널에서 실행되도록하십시오. React Native 프로젝트 폴더에서 새 터미널을 엽니다. 다음을 실행하십시오:

```shell
npx react-native run-ios
```

곧 iOS 시뮬레이터에서 새 앱이 실행되는 것을 볼 수 있습니다.

![AwesomeProject on iOS](https://reactnative.dev/assets/images/GettingStartediOSSuccess-e6dd7fc2baa303d1f30373d996a6e51d.png)

`npx react-native run-ios`는 앱을 실행하는 한 가지 방법입니다. Xcode 내에서 직접 실행할 수도 있습니다.

> 이 기능이 작동하지 않으면 [Troubleshooting](https://reactnative.dev/docs/troubleshooting#content) 페이지를 참조하세요.

### 디바이스에서 실행하기

위의 명령은 기본적으로 iOS 시뮬레이터에서 앱을 자동으로 실행합니다. 실제 실제 iOS 기기에서 앱을 실행하려면 [here](https://reactnative.dev/docs/running-on-device) 안내를 따르세요.

### 앱 수정하기

이제 앱을 성공적으로 실행했으므로 수정해 보겠습니다.

-선택한 텍스트 편집기에서 `App.js`를 열고 몇 줄을 편집합니다.

-iOS Simulator에서 `⌘R`를 눌러 리로드하고 변경 사항을 확인하세요!

### 끝!

축하합니다! 첫 번째 React Native 앱을 성공적으로 실행하고 수정했습니다.

![img](https://reactnative.dev/docs/assets/GettingStartedCongratulations.png)

## 이제 무엇을 할까요?

-이 새로운 React Native 코드를 기존 애플리케이션에 추가하려면 [통합 가이드](https://reactnative.dev/docs/integration-with-existing-apps)를 확인하세요.

React Native에 대해 자세히 알고 싶다면 [Introduction to React Native](https://reactnative.dev/docs/getting-started)를 확인하세요.
