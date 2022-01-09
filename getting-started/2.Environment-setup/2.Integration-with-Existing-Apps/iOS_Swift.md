# 기존 앱과의 통합

새 모바일 앱을 처음부터 시작할 때 Ract Native가 좋습니다. 그러나 기존 native 어플리케이션에 single view 또는 사용자 흐름을 추가하는 데도 유용합니다. 몇 단계로 새로운 React Native 기반 features, screens, view 등을 추가할 수 있습니다.

구체적인 단계는 대상 플랫폼에 따라 다릅니다.

- Android (Java)
- iOS (Objective-C)
- **iOS (Swift)**

## 핵심 개념

React Native 구성 요소를 iOS 애플리케이션에 통합하기위한 핵심은 다음과 같습니다.

React Native 종속성 및 디렉터리 구조를 설정합니다.
앱에서 사용할 React Native 구성 요소를 이해합니다.
CocoaPods를 사용하여 이러한 구성 요소를 종속성으로 추가합니다.
JavaScript로 React Native 구성 요소를 개발합니다.
iOS 앱에RCTRootView를 추가합니다. 이 뷰는 React Native 구성 요소의 컨테이너 역할을합니다.
React Native 서버를 시작하고 기본 애플리케이션을 실행합니다.
애플리케이션의 React Native 부분이 예상대로 작동하는지 확인합니다.

## 전제 조건

iOS 용 React Native 앱 빌드를위한 개발 환경을 구성하려면 [환경 설정 가이드](https://reactnative.dev/docs/environment-setup)의 React Native CLI 빠른 시작을 따르세요.

### 1. 디렉터리 구조 설정하기

원활한 진행을 위해서 통합된 React Native 프로젝트를 위한 새 폴더를 만든 다음 기존 iOS 프로젝트를 `/ios`하위 폴더에 복사합니다.

### 2. JavaScript 종속성 설치

프로젝트의 루트 디렉터리로 이동하여 다음 내용으로 새 `package.json` 파일을 만듭니다.

```json
{
  "name": "MyReactNativeApp",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "yarn react-native start"
  }
}
```

[installed the yarn package manager](https://yarnpkg.com/lang/en/docs/install/)를 진행했는지 확인합니다.

`react` 및`react-native` 패키지를 설치합니다. 터미널 또는 명령 프롬프트를 열고`package.json` 파일이있는 디렉토리로 이동하여 다음을 실행합니다.

```shell
$ yarn add react-native
```

그러면 다음과 유사한 메시지가 출력됩니다(yarn 출력에서 위로 스크롤하여 확인).

> warning "[react-native@0.52.2](mailto:react-native@0.52.2)" has unmet peer dependency "[react@16.2.0](mailto:react@16.2.0)".

괜찮습니다. 이는 React를 설치해야 함을 의미합니다.

```shell
$ yarn add react@version_printed_above
```

yarn이 새로운`/node_modules` 폴더를 생성했습니다. 이 폴더는 프로젝트를 빌드하는 데 필요한 모든 JavaScript 종속성을 저장합니다.

`.gitignore` 파일에`node_modules/`를 추가합니다.

### 3. CocoaPods 설치

[CocoaPods](http://cocoapods.org/)는 iOS 및 macOS 개발을위한 패키지 관리 도구입니다. 이를 사용하여 실제 React Native 프레임 워크 코드를 현재 프로젝트에 로컬로 추가합니다.

[Homebrew](http://brew.sh/)이용하여 CocoaPods를 설치하는 것을 추천합니다.

```shell
$ brew install cocoapods
```

> 기술적으로 CocoaPods를 사용하지 않는 것이 가능하지만 수동 라이브러리 및 링커 추가가 필요하므로 이 프로세스가 지나치게 복잡해집니다.

## 앱에 React Native 추가하기

[app for integration](https://github.com/JoelMarcey/swift-2048)이 [2048](<https://en.wikipedia.org/wiki/2048_(video_game)>)게임이라고 가정합니다. 다음은 React Native가 없는 네이티브 애플리케이션의 메인 메뉴입니다.

![Before RN Integration](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAPoAAAG8CAIAAAB4+C+vAAAJ4klEQVR4AezSMQEAIAACMBsTn1N7yJZhZwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAF2boju6gO/zWvW2SR44ZQCwWRFF4VUmUJCklihQigRYQUUgkKBAoIFGslCAQRBKwKoIqhRIlIahUQgoKgUAgRagU/9EwCCr8SBd89850TWfOzPOey+Xq9XqvN8pms7Va7fX51+s1Ho/rdDoOh8Plcg0GQzqd/lAR9/u91+t1Op3tdvth6Ha7hUIhh8NRLBZpMRwO2+32ZDIJpnE6nf7dIxqNTqdTUkTaaDTonHq9jsoX2nS328Ee/9+JQCDw3O6XywX+2263YKvV+ksW7Pf7eEtWKBSVSgW+xwHL5XJSqZTJZC6Xy4/bDPyXWCw2mUyw/m63i8pisTgejwC9Xm+z2UajEYvFKpVKqPy9x2azUSqVfr+fNjkcDqSPz+djs9mQiHRmMBh0jlAo/M6vC9VqFXL9eSfW6/VzuweDwcFgQFOyQ6lUSiwWy+Xy2WyGVKvVms1mk8kExq2m0WiwGvyKjBYKBZlMplKpVqsVUqPRaLFYBAIBmEa5XG42m4BOp+N2uz0ez3g8RopDORwOP9HuuH4AfD4/n88DoEkmkyH7dD6fAXh8RSIRgFqtbrVagEQiAXEe7E4YtwyPxyOdJRIJEWc+n4tEoh92zABSYSgKw+aFCgHwSEEAAIgIQgBJQAgAAgABAXggBCEEigAghABQQAMYqKQSUgj1Pv1ctN7aeDDt4Dn3bm9ru9/9z3/2sbgDVVDcKbbb7dYL91Kp5L7ZaDRSXQZiEsuyTqeTDqHQSqbTKfJGWdc5t9uNtSHJZrOXy+WlZUqlUmyDcrnMTePxuC4VxlgsFmgwr6VWq708YblccvRwOJCj+og3w0wmQ2V7wp0ZTkBKsEbCHReEsJEXi0WIj3D3j3u324UrL9yr1ar7Zp1Oh7VBZhB4hrgdc0irYnBHm1G470dQ2YX7X88wHo/ZFWY4m83CuBKIN6zbtr3f7zEbuBG3s8fJrNdrDVGBwWBwPB5x4fi3J9wVWCDERbhTJNkbm80Gved1RbgHwj2fz3vhjnvBWhgfL7lCbCTYb3F3HAfropnVauWNO9Hr9fTj1JyFMTDZ7HDt20ajgQaToMd4FRJeCFqOGzE1jYelKyVHGsjdZsaEwZ1FYSPx/YCZCPf/9O7qpdLpdL1eR7RUf1kwFhI/+hZ3NWeFQiGXyzWbzbe4qyFutVr3MEcymYzFYolEwrIstdrG7MnkfD2CJ2WmUqkwyb/wt9/v+8H9er1ykfsjPhP3yWTSbrcD4T6fz33hLhFyHOfpS1Cg+q5a/Dmx2+0kJ37ifD7btq0WNgqfMRwOf4KEPpy8wP1Xo2AUjM6qAtilExIAABgIQP077wPW4tAMgu6gO+gOuoPuoDvoDrqD7qA7uoPuoDvoDrqD7qA76A66g+6gO7qD7qA76A66g+6gO+gOuoPuoDu6g+6gO+gOuoPuoDvoDrqD7qA7uoPuoDvoDrqD7qA76A66g+6gO7qD7qA76A66g+6gO+gOuoPuoDvoju6gO+gOuoPuoDvoDrqD7qA76I7uoDvoDrqD7qA76A66g+6gO+iO7qA76A66g+6gO+gOuoPuoDvoju6gO+gOuoPuoDvoDrqD7qA76A66ozvoDrqD7qA76A66g+6gO+gOuqM76A66g+6gOzzdQXfQHXQH3UF3dAfdQXfQHXQH3UF30B10B91Bd3QH3UF30B10B91Bd9AddAfdQXd0B91Bd9AddAfdQXfQHXQH3UF3dAfdQXfQHXQH3UF30B10B91Bd9Ad3UF30B10B91Bd9AddAfdQXfQHd1Bd9AddAfdQXfQHXQH3UF30B3dQXfQnerZMLpz7JljnPRGHIB7ta1vtW3btvW1Nhevbdu2z7ZtW7GTSTKTzt60ua3Nd57f4o+Nnw1NG96+yCav59aCARmQekzwd+o+INvXzTFPH21cNctKrZM9CtX934MO4AEBr6BZxK+xe5kDg6i21/zduhe3qQcF4aIMDsfZDeJRI5yNxdR4qvu/THefDzb2PLvaiNa9k7Nuna/fOM9q6NNw+tKKwYwmg7TaGPOBhbwXxeljzQ0FnJ/W9OgHBj1CYpVw7Rxw+3yFUwCpPLSI72SN2+err65icTp2T//ls93p8X3DM7Op78o57qg9Ax6F6v5X6P7Zlu6nVg3rzirg0JC7Jo+PrRAODqG6Xr28S790ikB+/PEOafQ+xh/WJKP6MfLbzBNGmnvK+DW53IEBaLsoMomA9+BiJa9JumyadvZYYVICH1chHBTy0usV3L1ipvnGRq24VX5wiX7rLNaj/Lm6U91jy/kDg6ikXYneu0P4TXDNFGZOhgoR8lsxASTojj8qXnP9UZV26FdPGcSva6YM4hQhz+e0kbJioaFJeMCBOKjuVs6caJDuLdN7Z2dZouEeGIy0yLAHBnBM+ZN0p7rfMFPErxfXqGWdWvSlKkLo9rkyNu+4sH5Y0J6doeLik6uMDfkMq4BzJ5vfu+r1dWcVe2uxhF8xQxXRgBdOjpzWnxDWDg5C2YRk706o71UvmGKQ+J6Z3bOzwYACseJHhwz/ZdmuR/lTdKe6fw9f95fXqS+vEUnlvAkS0b2hVztnivvcKmVeKvO9oU4bg/8JrJ829RsHhRAOTgwrKQ0WKR4b1n9Rd0Ef3rtjIELeXw3Vner+0XbhoqkGq4K1+eJRIZPojsH7aXzXxfsBBa14F45mpvA6cHcUs8eMAMm1Mq5fOpF9Za2kAWdkrHRo0P5F3XFwzmTz/R2qAdzRscKBwT9j1051p5gAXjf/R+rXL0AkeGMNe/40J7Cb+2DzwMYSnRQ/3tJ/4wzW+zEYxb5sJjh9rHXFbFTarpKiA9H9S4yLZzjr87lnVkg6QJFJfDvddsZ4do1F4rc2DG4qt0n85DLxvGn2o8vUf/KZF9WdcmQYVPda3v8bqjvFcuDJo/R3dxh0VXzdLh2QAAAAIADq/+puROgGdQfdQXfQHXQH3UF3dAfdQXfQHXQH3UF30B10B91Bd3QH3UF30B10B91Bd9AddAfdQXd0B91Bd9AddAfdQXfQHXQH3UF3dAfdQXfQHXQH3UF30B10B91Bd9Ad3UF30B10B91Bd9AddAfdQXfQHd1Bd9AddAfdQXfQHXQH3UF30B3dQXfQHXQH3UF30B10B91Bd9Ad3UF30B10B91Bd9AddAfdQXfQfQm6g+6gO+gOuoPu6A66g+6gO+gOuoPuoDvoDrqD7ugOuoPuoDvoDrqD7qA76A66g+7oDrqD7qA76A66g+6gO+gOuoPu6A66g+6gO+gOuoPuoDvoDrqD7pB/AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABSFMFhbxARPwgAAAABJRU5ErkJggg==)

### Xcode용 명령 행 도구

명령 줄 도구를 설치합니다. Xcode 메뉴에서 "Preferences..."를 선택하십시오. 위치 패널로 이동하고 명령 줄 도구 드롭 다운에서 최신 버전을 선택하여 도구를 설치합니다.

![Xcode Command Line Tools](https://reactnative.dev/assets/images/GettingStartedXcodeCommandLineTools-8259be8d3ab8575bec2b71988163c850.png)

### CocoaPods 종속성 설정하기

React Native를 애플리케이션에 통합하기 전에 React Native 프레임 워크의 어떤 부분을 통합할지 결정해야합니다. CocoaPods를 사용하여 앱이 의존 할 이러한 "subspecs"을 지정합니다.

지원되는`subspec` 목록은 [`/node_modules/react-native/React.podspec`](https://github.com/facebook/react-native/blob/master/React.podspec)에서 확인할 수 있습니다. 일반적으로 기능에 따라 이름이 지정됩니다. 예를 들어, 일반적으로 항상`Core` `subspec`이 필요합니다. 그러면 `AppRegistry`,`StyleSheet`,`View` 및 기타 핵심 React Native 라이브러리가 제공됩니다. React Native `Text` 라이브러리 (예 :`<Text>`요소 용)를 추가하려면`RCTText` `subspec`이 필요합니다. `Image` 라이브러리 (예 :`<Image>`요소의 경우)를 원한다면`RCTImage` `subspec`이 필요합니다.

앱이 'Podfile'파일에 의존 할 `subspec`을 지정할 수 있습니다. `Podfile`을 만드는 가장 쉬운 방법은 프로젝트의`/ios` 하위 폴더에서 CocoaPods` init` 명령을 실행하는 것입니다.

```shell
$ pod init
```

`Podfile`에는 통합 목적으로 조정할 상용구 설정이 포함됩니다.

> `Podfile` 버전은`react-native` 버전에 따라 변경됩니다. 사용해야 하는`Podfile`의 특정 버전은 https://react-native-community.github.io/upgrade-helper/ 를 참조하십시오.

궁극적으로`Podfile`은 다음과 유사해야합니다:

```json
source 'https://github.com/CocoaPods/Specs.git'

# Required for Swift apps
platform :ios, '8.0'
use_frameworks!

# The target name is most likely the name of your project.
target 'swift-2048' do

  # Your 'node_modules' directory is probably in the root of your project,
  # but if not, adjust the `:path` accordingly
  pod 'React', :path => '../node_modules/react-native', :subspecs => [
    'Core',
    'CxxBridge', # Include this for RN >= 0.47
    'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
    'RCTText',
    'RCTNetwork',
    'RCTWebSocket', # needed for debugging
    # Add any other subspecs you want to use in your project
  ]
  # Explicitly include Yoga if you are using RN >= 0.42.0
  pod "Yoga", :path => "../node_modules/react-native/ReactCommon/yoga"

  # Third party deps podspec link
  pod 'DoubleConversion', :podspec => '../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
  pod 'glog', :podspec => '../node_modules/react-native/third-party-podspecs/glog.podspec'
  pod 'Folly', :podspec => '../node_modules/react-native/third-party-podspecs/Folly.podspec'

end
```

`Podfile`을 생성했으면 React Native pod를 설치할 준비가되었습니다.

```shell
$ pod install
```

다음과 같은 출력을 확인할 수 있습니다:

```json
Analyzing dependencies
Fetching podspec for `React` from `../node_modules/react-native`
Downloading dependencies
Installing React (0.62.0)
Generating Pods project
Integrating client project
Sending stats
Pod installation complete! There are 3 dependencies from the Podfile and 1 total pod installed.
```

> 이것이`xcrun`을 언급하는 오류와 함께 실패하면 **Preferences > Locations**의 Xcode에서 Command Line Tools가 할당되었는지 확인합니다.

> "_The `swift-2048 [Debug]` target overrides the `FRAMEWORK_SEARCH_PATHS` build setting defined in `Pods/Target Support Files/Pods-swift-2048/Pods-swift-2048.debug.xcconfig`. This can lead to problems with the CocoaPods installation_"와 같은 오류가 출력된다면, `Debug` 및 `Release` 모두에 대한 `Build Settings`의 `Framework Search Paths`에 `$(inherited)`만 포함되어 있는지 확인합니다.

### 코드 통합

이제 실제로 네이티브 iOS 애플리케이션을 수정하여 React Native를 통합합니다. 2048 샘플 앱의 경우 React Native에 "High Score"화면을 추가합니다.

#### React Native 컴포넌트

우리가 작성할 첫 번째 코드는 애플리케이션에 통합 될 새로운 "High Score" 화면에 대한 실제 React Native 코드입니다.

##### 1. `index.js` 파일 생성하기

먼저 React Native 프로젝트의 루트에 빈`index.js` 파일을 만듭니다.

`index.js`는 React Native 애플리케이션의 시작점이며 항상 필요합니다. React Native 구성 요소 또는 애플리케이션의 일부인 다른 파일을 `require` 하는 작은 파일이나 필요한 모든 코드를 포함 할 수 있습니다. 우리의 경우 모든 것을 `index.js`에 넣을 것입니다.

##### 2. React Native 코드 추가하기

`index.js`에서 구성 요소를 만듭니다. 여기 샘플에서는 스타일이 지정된`<View>`내에`<Text>`구성 요소를 추가합니다.

```jsx
import React from "react";
import { AppRegistry, StyleSheet, Text, View } from "react-native";

class RNHighScores extends React.Component {
  render() {
    var contents = this.props["scores"].map((score) => (
      <Text key={score.name}>
        {score.name}:{score.value}
        {"\n"}
      </Text>
    ));
    return (
      <View style={styles.container}>
        <Text style={styles.highScoresTitle}>2048 High Scores!</Text>
        <Text style={styles.scores}>{contents}</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    backgroundColor: "#FFFFFF",
  },
  highScoresTitle: {
    fontSize: 20,
    textAlign: "center",
    margin: 10,
  },
  scores: {
    textAlign: "center",
    color: "#333333",
    marginBottom: 5,
  },
});

// Module name
AppRegistry.registerComponent("RNHighScores", () => RNHighScores);
```

> `RNHighScores`는 iOS 애플리케이션 내에서 React Native에 뷰를 추가 할 때 사용할 모듈의 이름입니다.

#### `RCTRootView`의 마법

이제 React Native 구성 요소가 `index.js`를 통해 생성되었으므로 해당 구성 요소를 신규 또는 기존 `ViewController`에 추가해야합니다. 가장 쉬운 방법은 선택적으로 구성 요소에 대한 이벤트 경로를 만든 다음 해당 구성 요소를 기존 `ViewController`에 추가하는 것입니다.

React Native 구성 요소를 실제로 `RCTRootView`라는 이름을 포함하는 `ViewController`의 새 네이티브 뷰와 연결합니다.

##### 1. 이벤트 Path 생성하기

메인 게임 메뉴에 새로운 링크를 추가하여 "High Score"React Native 페이지로 이동할 수 있습니다.

![Event Path](data:image/png;base64,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fh9SKZfLzWazVqsJLsAD4/G42+1Wq9WQym63S3bo9XqCC/BAHMdJbYfD4Xq9Dp9Ur9cHg0Gyw2w2i6KoVCo5wwV44L22qVf1+/3wDQQX4HQ6CS4Aggs8kWKxKLgAH/r89cVVo9HIlALAPYVCYbvdJsMGIZVkbbJDq9UK9wkuQLvdnkwmcRyHVPL5fKfTCc/M84zAD/A8IwA+mgEILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIILgOACCC4AggsguACCC4DgAgguAIILILgAgguA4AIILgCCCyC4AIL75AAEF0BwARBcAMEFEFwABBdAcAEQXADBBRBcAAQXQHABEFwAwQUQXAAEF0BwARBcAMEFEFwABBdAcAEQXADBBRBcAAQXQHABEFwAwQUQXAAEF0BwARBcAMEFEFwABBdAcAEQXADBBRBcAAQXQHABEFwAwQUQXAAEF0BwARBcAMEFEFwABBdAcAEQXADBBRBcAAQXQHABEFwAwQUQXAAEF0BwARBcAMEFEFwABBdAcAEQXADBBRBcAAQXQHABEFwAwQUQXAAEF0BwARBcAMEFEFwABPd4PF6v1wAguJk6HA7L5XKxWGguILgZulwuq9Xqdrudz2fNBQQ3Q7lcLoqit9+aCwhutiqVSqPR0FxAcDUXIEVwNRdAcDUXEFzNBUgfXM0FEFzN5ZV9uwBvIs3jOP5m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)

##### 2. 이벤트 핸들러

이제 메뉴 링크에서 이벤트 핸들러를 추가합니다. 애플리케이션의 메인 `ViewController`에 메소드가 추가됩니다. 이것이 `RCTRootView`가 작동하는 곳 입니다.

React Native 애플리케이션을 빌드 할 때 [Metro bundler](https://facebook.github.io/metro/)를 사용하여 React Native 서버에서 제공 할 `index.bundle`을 만듭니다. `index.bundle` 내부에는`RNHighScore` 모듈이 있습니다. 따라서 `RCTRootView`가 `index.bundle` 리소스의 위치(`NSURL`을 통해)를 가리키고 모듈에 연결해야합니다.

디버깅 목적으로 이벤트 핸들러가 호출되었음을 기록합니다. 그런 다음 `index.bundle` 내에 존재하는 React Native 코드의 위치로 문자열을 생성합니다. 마지막으로 메인 `RCTRootView`를 생성합니다. React Native 컴포넌트에 대한 코드를 작성할 때 [위](https://reactnative.dev/docs/integration-with-existing-apps#the-react-native-component)에서 생성한 `moduleName`으로 `RNHighScores`를 제공하는 방법에 주목하십시오.

먼저 `React` 라이브러리를 `import`합니다.

```jsx
import React
```

> `initialProperties`는 예시 목적으로 여기에 있으므로 높은 점수 화면에 대한 데이터가 있습니다. React Native 컴포넌트에서 `this.props`를 사용하여 해당 데이터에 액세스합니다.

```swift
@IBAction func highScoreButtonTapped(sender : UIButton) {
  NSLog("Hello")
  let jsCodeLocation = URL(string: "http://localhost:8081/index.bundle?platform=ios")
  let mockData:NSDictionary = ["scores":
      [
          ["name":"Alex", "value":"42"],
          ["name":"Joel", "value":"10"]
      ]
  ]

  let rootView = RCTRootView(
      bundleURL: jsCodeLocation,
      moduleName: "RNHighScores",
      initialProperties: mockData as [NSObject : AnyObject],
      launchOptions: nil
  )
  let vc = UIViewController()
  vc.view = rootView
  self.present(vc, animated: true, completion: nil)
}
```

> `RCTRootView bundleURL`은 새 JSC VM을 시작합니다. 리소스를 절약하고 네이티브 앱의 다른 부분에서 RN 뷰 간의 통신을 단순화하기 위해 단일 JS 런타임과 연결된 React Native에서 제공하는 여러 뷰를 가질 수 있습니다. 이렇게하려면 `RCTRootView bundleURL`을 사용하는 대신 [`RCTBridge initWithBundleURL`](https://github.com/facebook/react-native/blob/master/React/Base/RCTBridge.h#L89)를 사용하여 브리지를 사용한 다음`RCTRootView initWithBridge`를 사용합니다.

> 앱을 프로덕션으로 이동할 때 `NSURL`은 `let mainBundle = NSBundle(URLForResource: "main" withExtension:"jsbundle")`과 같은 것을 통해 디스크의 사전 번들 파일을 가리킬 수 있습니다. `node_modules/react-native/scripts/`의`react-native-xcode.sh` 스크립트를 사용하여 사전 번들 파일을 생성 할 수 있습니다.

##### 3. 연결하기

메인 메뉴의 새 링크를 새로 추가 된 이벤트 핸들러 메서드에 연결합니다.

![Event Path](https://reactnative.dev/assets/images/react-native-add-react-native-integration-wire-up-37137857e0876d2aca7049db6d82fcb6.png)

> 이를 수행하는 더 쉬운 방법 중 하나는 스토리 보드에서 뷰를 열고 새 링크를 마우스 오른쪽 단추로 클릭하는 것입니다. `Touch Up Inside`이벤트와 같은 것을 선택하고 스토리 보드로 드래그 한 다음 제공된 목록에서 생성 된 방법을 선택합니다.

### 통합 테스트

이제 React Native를 현재 애플리케이션과 통합하기 위한 모든 기본 단계를 완료했습니다. 이제 [Metro bundler](https://facebook.github.io/metro/)를 시작하여`index.bundle` 패키지를 빌드하고`localhost`에서 실행되는 서버를 제공합니다.

##### 1. 앱 전송 보안(App Transport Security, ATS) 예외 추가하기

Apple은 implicit cleartext HTTP 리소스로드를 차단했습니다. 따라서 다음 프로젝트의 `Info.plist` (또는 이와 동등한) 파일을 추가해야합니다.

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>localhost</key>
        <dict>
            <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
            <true/>
        </dict>
    </dict>
</dict>
```

> 앱 전송 보안은 사용자에게 좋습니다. 프로덕션 용으로 앱을 출시하기 전에 다시 활성화해야합니다.

##### 2. Packager 실행하기

앱을 실행하려면 먼저 개발 서버를 시작해야합니다. 이렇게하려면 React Native 프로젝트의 루트 디렉터리에서 다음 명령을 실행합니다.

```shell
$ npm start
```

##### 3. 앱 실행하기

Xcode 또는 선호하는 편집기를 사용하는 경우 기본 iOS 애플리케이션을 정상적으로 빌드하고 실행하십시오. 또는 다음을 사용하여 명령 줄에서 앱을 실행할 수 있습니다.

```shell
# From the root of your project
$ npx react-native run-ios
```

샘플 애플리케이션에서 "High Scores"에 대한 링크를 확인한 다음 이를 클릭하면 React Native 구성 요소의 렌더링을 볼 수 있습니다.

_native_ 애플리케이션 홈 화면은 다음과 같습니다:

![Home Screen](data:image/png;base64,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)

_React Native_ 의 high score 스크린입니다:

![High Scores](data:image/png;base64,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)

> 애플리케이션 실행시 module resolution 문제가 발생하는 경우 [this GitHub issue](https://github.com/facebook/react-native/issues/4968)에서 정보와 가능한 해결 방법을 참조하세요. [This comment](https://github.com/facebook/react-native/issues/4968#issuecomment-220941717)이 가능한 최신 해결 방법 인 것 같습니다.

### 코드 보기

샘플 앱에 React Native 화면을 추가 한 코드는 [GitHub](https://github.com/JoelMarcey/swift-2048/commit/13272a31ee6dd46dc68b1dcf4eaf16c1a10f5229)에서 확인할 수 있습니다.

### 이제 무엇을 할까요?

이 시점에서 평소처럼 앱을 계속 개발할 수 있습니다. React Native 작업에 대한 자세한 내용은 [debugging](https://reactnative.dev/docs/debugging) 및 [deployment](https://reactnative.dev/docs/running-on-device) 문서를 참조하세요.
