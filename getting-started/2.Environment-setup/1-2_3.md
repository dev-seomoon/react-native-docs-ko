<-- Tab: React Native CLI Quickstart -->

<-- Inner Tab: Windows/Android -->

# 개발 환경 설정

이 페이지는 React Native 앱을 설치하고 빌드하는 첫 걸음에 도움이 됩니다.

**모바일 개발이 처음인 경우**, 시작하는 가장 쉬운 방법은 Expo CLI를 사용하는 것입니다. Expo는 React Native를 중심으로 구축 된 도구 모음이며 [features](https://expo.io/features)이 많지만 현재 가장 관련성이 높은 기능은 몇 분 안에 React Native 앱을 작성할 수 있다는 것입니다. 최신 버전의 Node.js와 핸드폰 또는 에뮬레이터만 있으면 됩니다. 도구를 설치하기 전에 웹 브라우저에서 직접 React Native를 사용해보고 싶다면 [Snack](https://snack.expo.io/)를 사용해보세요.

**이미 모바일 개발에 익숙한 경우**, React Native CLI를 사용할 수 있습니다. 시작하려면 Xcode 또는 Android Studio가 필요합니다. 이러한 도구 중 하나가 이미 설치되어있는 경우 몇 분 내에 시작하여 실행할 수 있습니다. 설치되지 않은 경우 설치 및 구성하는 데 약 1시간이 소요됩니다.

## React Native CLI 빠르게 시작하기

프로젝트에서 네이티브 코드를 빌드해야하는 경우 다음 지침을 따르세요. 예를 들어 React Native를 기존 애플리케이션에 통합하거나 Expo에서 "ejected"한 경우 이 섹션이 필요합니다.

지침은 개발 운영 체제 및 iOS 또는 Android 용 개발을 시작할지 여부에 따라 약간 다릅니다. Android와 iOS 공용으로 개발하고 싶다면 괜찮습니다. 설정이 약간 다르기 때문에 어떤 것을 시작할지 하나를 선택할 수 있습니다.

#### 개발 OS: Windows

#### 타겟 OS: Android

## 종속성 설치

Node, React Native command line 인터페이스, JDK 및 Android Studio가 필요합니다.

선택한 편집기를 사용하여 앱을 개발할 수 있지만 Android 용 React Native 앱을 빌드하는 데 필요한 도구를 설정하려면 Android Studio를 설치해야합니다.

### Node 및 JDK

Windows 용으로 널리 사용되는 패키지 관리자인 [Chocolatey](https://chocolatey.org/)을 통해 Node를 설치하는 것이 좋습니다.

다른 노드 버전 간에 전환하려면 Windows 용 노드 버전 관리자인 [nvm-windows](https://github.com/coreybutler/nvm-windows)를 통해 Node를 설치하는 것이 좋습니다.

React Native 에는 [Java SE Development Kit (JDK)](https://openjdk.java.net/projects/jdk8/)도 필요하며 Chocolatey를 사용하여 설치할 수도 있습니다.

관리자 명령 프롬프트를 열고 (명령 프롬프트를 마우스 오른쪽 단추로 클릭하고 "관리자 권한으로 실행"선택) 다음 명령을 실행합니다:

```powershell
choco install -y nodejs.install openjdk8
```

시스템에 Node를 이미 설치 한 경우 Node 12 이상인지 확인하십시오. 시스템에 이미 JDK가 있는 경우 버전 8 이상인지 확인하십시오.

> [Node's Downloads page](https://nodejs.org/en/download/)에서 추가 설치 옵션을 찾을 수 있습니다.

> 최신 버전의 Java Development Kit를 사용하는 경우 JDK를 인식 할 수 있도록 프로젝트의 Gradle 버전을 변경해야 합니다. `{project root folder}\android\gradle\wrapper\gradle-wrapper.properties`로 이동하고 `distributionUrl`값을 변경하여 Gradle 버전을 업그레이드하면됩니다. [here the lastest releases of Gradle](https://gradle.org/releases/)를 확인할 수 있습니다.

### Android 개발 환경

Android 개발을 처음 접하는 경우 개발 환경 설정이 다소 지루할 수 있습니다. Android 개발에 이미 익숙한 경우 구성해야 할 몇 가지 사항이 있습니다. 두 경우 모두 다음 몇 단계를 주의 깊게 따르십시오:

#### 1. Android Studio 설치

[Download and install Android Studio](https://developer.android.com/studio/index.html). Android Studio 설치 마법사에서 다음 모든 항목 옆의 확인란이 선택되어 있는지 확인합니다:

- `Android SDK`
- `Android SDK Platform`
- `Android Virtual Device`
- If you are not already using Hyper-V: `Performance (Intel ® HAXM)` ([See here for AMD or Hyper-V](https://android-developers.googleblog.com/2018/07/android-emulator-amd-processor-hyper-v.html))

그런 다음 "다음"을 클릭하여 이러한 구성 요소를 모두 설치합니다.

> 확인란이 회색으로 표시되면 나중에 이러한 구성 요소를 설치할 수 있습니다.

설정이 완료되고 시작 화면이 표시되면 다음 단계로 진행합니다.

#### 2. Android SDK 설치

Android Studio는 기본적으로 최신 Android SDK를 설치합니다. 그러나 네이티브 코드로 React Native 앱을 빌드하려면 특히`Android 10 (Q)` SDK가 필요합니다. Android Studio의 SDK Manager를 통해 추가 Android SDK를 설치할 수 있습니다.

이를 위해 Android Studio를 열고 "Configure" 버튼을 클릭 한 다음 "SDK Manager"를 선택합니다.

![Android Studio Welcome](https://reactnative.dev/assets/images/GettingStartedAndroidStudioWelcomeWindows-b88d46e9a7fe5e050224a9a295148222.png)

> SDK Manager는 **Appearance & Behavior** → **System Settings** → **Android SDK** 아래의 Android Studio "Preferences" 대화 상자에서도 찾을 수 있습니다.

SDK Manager 내에서 "SDK Platforms" 탭을 선택한 다음 오른쪽 하단의 "Show Package Details" 옆에있는 확인란을 선택합니다. `Android 10 (Q)`항목을 찾아 확장 한 후 다음 항목이 선택되었는지 확인합니다:

- `Android SDK Platform 29`
- `Intel x86 Atom_64 System Image` or `Google APIs Intel x86 Atom System Image`

그런 다음 "SDK Tools" 탭을 선택하고 여기에서 "Show Package Details" 옆의 확인란도 선택합니다. "Android SDK Build-Tools" 항목을 찾아 펼친 다음, `29.0.2`이 선택되어 있는지 확인합니다.

마지막으로 "Apply" 을 클릭하여 Android SDK 및 관련 빌드 도구를 다운로드하고 설치합니다.

#### 3. ANDROID_HOME 환경 변수 설정

React Native 도구는 네이티브 코드로 앱을 빌드하기 위해 몇 가지 환경 변수를 설정해야합니다.

1. **Windows Control Panel.** 열기
2. **User Accounts,** 클릭 후 **User Accounts** 다시 클릭
3. **Change my environment variables** 클릭
4. **New...** 을 클릭하여 Android SDK의 경로를 가리키는 `ANDROID_HOME` 사용자 변수를 생성하기:

![ANDROID_HOME Environment Variable](data:image/png;base64,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)

SDK는 기본적으로 다음 위치에 설치됩니다:

```powershell
%LOCALAPPDATA%\Android\Sdk
```

**Appearance & Behavior** → **System Settings** → **Android SDK**.아래의 Android Studio "Settings" 대화 상자에서 SDK의 실제 위치를 찾을 수 있습니다.

다음 단계로 진행하기 전에 새 환경 변수가 로드되었는지 확인하려면 새 명령 프롬프트 창을 엽니다.

1. powershell 열기
2. \*\*Get-ChildItem -Path Env:\*\* 를 powershell에 복사하여 붙여넣기
3. `ANDROID_HOME`이 추가되었는지 확인

#### 4. Path에 platform-tools 추가

1. **Windows Control Panel.** 열기
2. **User Accounts,** 클릭 후 **User Accounts** 다시 클릭
3. **Change my environment variables** 클릭
4. **Path** 변수 선택
5. **Edit.** 클릭
6. **New** 클릭 후 목록에 platform-tools 경로 추가

이 폴더의 기본 위치는 다음과 같습니다:

```powershell
%LOCALAPPDATA%\Android\Sdk\platform-tools
```

### React Native 명령 행 도구

React Native에는 기본 제공 command line 인터페이스가 있습니다. CLI의 특정 버전을 전역적으로 설치하고 관리하는 대신 Node.js와 함께 제공되는 `npx`를 사용하여 런타임에 현재 버전에 액세스하는 것이 좋습니다. `npx react-native <command>`를 사용하면 명령이 실행될 때 현재 안정된 버전의 CLI가 다운로드되고 실행됩니다.

## 새 애플리케이션 생성하기

> 이전에 글로벌 `react-native-cli` 패키지를 설치 한 경우 예기치 않은 문제가 발생할 수 있으므로 제거하십시오.

React Native에는 새 프로젝트를 생성하는 데 사용할 수있는 기본 제공 command line 인터페이스가 있습니다. Node.js와 함께 제공되는 `npx`를 사용하여 전역 적으로 설치하지 않고도 액세스 할 수 있습니다. "AwesomeProject"라는 새로운 React Native 프로젝트를 만들어 보겠습니다:

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

## Android 디바이스 준비하기

React Native Android 앱을 실행하려면 Android 기기가 필요합니다. 이것은 실제 Android 기기 일 수도 있고, 더 일반적으로 컴퓨터에서 Android 기기를 에뮬레이션 할 수있는 Android 가상 기기를 사용할 수 있습니다.

어느 쪽이든 개발을 위해 Android 앱을 실행할 수 있도록 기기를 준비해야합니다.

### 물리적 디바이스 사용하기

실제 Android 기기가 있는 경우 USB 케이블을 사용하여 컴퓨터에 연결하고 [here](https://reactnative.dev/docs/running-on-device) 안내에 따라 AVD 기기 대신 개발에 사용할 수 있습니다.

### 가상 디바이스 사용하기

Android Studio를 사용하여 `./AwesomeProject/android`를 여는 경우 Android Studio 내에서 "AVD Manager"를 열어 사용 가능한 Android Virtual Devices(AVDs) 목록을 볼 수 있습니다. 다음과 같은 아이콘을 찾으십시오:

![Android Studio AVD Manager](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB0AAAAZCAIAAABCYLJOAAACLUlEQVR4Ab2Vw6IfPxiG3zfJHPPP2rbdfW3rProsLqCr7rqsbVur2raPPb+Z5Du2+YyRJ87HtLQ0dAIKnYNBQ5BEI4hI271BEIjUNZAgqbUm2WovyRLpyVMnfd83xggq3ATDMHTOzZo1q3ev3iRbXV4RKZEuXLBQayPOAQSESjlnDx85nJ+fjwpa7zUlaHPj2CO/oLTUNnTR8Wb2kvGeZ9rVbwJxTuArm69haAMaqtJXgnZ5y1tVmxKsMoqgKS0p2z1+WdEgItWXjp4XJEiCHesVZ50LnbUOrcE0LQURk2BsYHU0ASmnuqUgrfaSEAcvWs9eMQbKGe0J0L17j8TERJICW+hnx0Ylk6o17SAVF6U1lL3/bld+UbrAzpgx9f///4sEeT/TH955srMwkk2wNeUlROCsiBNCicu7dH+zH5BUgChapThh0NrYqCRpbTsoxZiEKBu60EbSsr+m5Z7RaoiIJQkQkv3lz8De/8/0tJEWekmKdSXb9MUjRYK7L3bk+996/7PpT/Zr6yIko0xcSsK0r2n33/+4NLjnXNIA0nz7kizy/b379ly6ckGIb+kPUhP7zZu5zTN/B/ZzaL+lxPecN2O7YsKbL2edOLLF5f3/v/+zsrOSk1MAxEb1z8j5dezaVggTYiYDyCnIP359q4gENl4EDdJwfLPWAiCplAptABEHp2oMKSfljzTaa0W/GVPxXkQ8EwWw5gCs+VhCK7w1/25o0ZHOj8dd7C0GRnwgNA5r8rwAAAAASUVORK5CYII=)

최근에 Android Studio를 설치했다면 [create a new AVD](https://developer.android.com/studio/run/managing-avds.html)이 필요할 수 있습니다. "Create Virtual Device..."를 선택한 다음 목록에서 기기를 선택하고 "Next"을 클릭 한 다음 **\*\*Q\*\*** API Level 29 이미지를 선택합니다.

> HAXM이 설치되어 있지 않은 경우 "Install HAXM"를 클릭하거나 [these instructions](https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows)에 따라 설치합니다. 그런 다음 AVD Manag로 돌아가십시오.

"Next"을 클릭 한 다음 "Finish"을 클릭하여 AVD를 만듭니다. 이 시점에서 AVD 옆에있는 녹색 삼각형 버튼을 클릭하여 시작하고 다음 단계로 진행할 수 있습니다.

## React Native 애플리케이션 실행하기

### 1단계: Metro 시작하기

먼저 React Native와 함께 제공되는 JavaScript 번들러 인 Metro를 시작해야합니다. Metro는 "입력 파일과 다양한 옵션을 가져 와서 모든 코드와 해당 종속성을 포함하는 단일 JavaScript 파일을 반환합니다."—[Metro Docs](https://facebook.github.io/metro/docs/concepts)

Metro를 시작하려면 React Native 프로젝트 폴더 내에서 `npx react-native start`를 실행하세요:

```shell
npx react-native start
```

`react-native start`은 Metro Bundler를 시작합니다.

> Yarn 패키지 관리자를 사용하는 경우 기존 프로젝트 내에서 React Native 명령을 실행할 때 `npx` 대신`yarn`을 사용할 수 있습니다.

> 웹 개발에 익숙하다면 Metro는 React Native 앱용 webpack과 매우 유사합니다. Kotlin 또는 Java와 달리 JavaScript는 컴파일되지 않으며 React Native도 아닙니다. 번들링은 컴파일과 동일하지 않지만 시작 성능을 향상시키고 일부 플랫폼 별 JavaScript를 더 광범위하게 지원되는 JavaScript로 변환하는 데 도움이 될 수 있습니다.

### 2단계: 애플리케이션 시작하기

Metro Bundler가 자체 터미널에서 실행되도록하십시오. React Native 프로젝트 폴더에서 새 터미널을 엽니다. 다음을 실행하십시오:

```shell
npx react-native run-android
```

모든 것이 올바르게 설정되면 곧 Android 에뮬레이터에서 새 앱이 실행되는 것을 볼 수 있습니다.

![AwesomeProject on Android](https://reactnative.dev/assets/images/GettingStartedAndroidSuccessWindows-7ae949ba8187936ba342678c432d78f6.png)

`npx react-native run-android`는 앱을 실행하는 한 가지 방법입니다. Android Studio 내에서 직접 실행할 수도 있습니다.

> 이 작업을 수행할 수 없는 경우 [Troubleshooting](https://reactnative.dev/docs/troubleshooting#content) 페이지를 확인하세요.

### 앱 수정하기

이제 앱을 성공적으로 실행했으므로 수정해 보겠습니다.

-선택한 텍스트 편집기에서 `App.js`를 열고 몇 줄을 편집합니다.

-`R` 키를 두 번 누르거나 개발자 메뉴 (`Ctrl + M`)에서`Reload`를 선택하여 변경 사항을 확인하세요!

### 끝!

축하합니다! 첫 번째 React Native 앱을 성공적으로 실행하고 수정했습니다.

![img](https://reactnative.dev/docs/assets/GettingStartedCongratulations.png)

## 이제 무엇을 하나요?

-이 새로운 React Native 코드를 기존 애플리케이션에 추가하려면 [통합 가이드](https://reactnative.dev/docs/integration-with-existing-apps)를 확인하세요.

React Native에 대해 자세히 알고 싶다면 [Introduction to React Native](https://reactnative.dev/docs/getting-started)를 확인하세요.
