# TV 기기를 위한 빌드

TV 장치 지원은 기존 React Native 응용 프로그램이 Apple TV 및 Android TV에서 작동하도록 구현되었으며 응용 프로그램의 JavaScript 코드를 거의 또는 전혀 변경하지 않아도됩니다.


> **사용되지 않음.** [react-native-tvos](https://github.com/react-native-community/react-native-tvos)를 대신 사용하세요. [0.62 release blog post](https://reactnative.dev/blog/#moving-apple-tv-to-react-native-tvos)에서 상세 내용을 확인할 수 있습니다.

## 빌드 변경[#](https://reactnative.dev/docs/building-for-tv#build-changes-1)

- *Native layer*: React Native Xcode 프로젝트는 이제 이름이 `-tvOS`문자열로 끝나는 Apple TV 빌드 대상을 갖습니다.
- *react-native init*: `react-native init`로 생성 된 새로운 React Native 프로젝트에는 XCode 프로젝트에서 Apple TV 대상이 자동으로 생성됩니다.
- *JavaScript layer*: Apple TV 지원이`Platform.ios.js`에 추가되었습니다. 다음을 수행하여 코드가 AppleTV에서 실행 중인지 확인할 수 있습니다.

```js
var Platform = require('Platform');
var running_on_tv = Platform.isTV;

// If you want to be more specific and only detect devices running tvOS
// (but no Android TV devices) you can use:
var running_on_apple_tv = Platform.isTVOS;
```

## 코드 변경[#](https://reactnative.dev/docs/building-for-tv#code-changes-1)

- *General support for tvOS*: 네이티브 코드의 Apple TV 특정 변경 사항은 모두 TARGET_OS_TV 정의에 의해 래핑됩니다. 여기에는 tvOS에서 지원되지 않는 API (예 : 웹보기, 슬라이더, 스위치, 상태 표시 줄 등)를 막기 위한 변경 사항과 TV 리모컨 또는 키보드에서 사용자 입력을 지원하기 위한 변경 사항이 포함됩니다.
- *Common codebase*: tvOS와 iOS는 대부분의 Objective-C 및 JavaScript 코드를 공통적으로 공유하므로 iOS에 대한 대부분의 문서는 tvOS에 동일하게 적용됩니다.
- *Access to touchable controls*: Apple TV에서 실행할 때 네이티브 뷰 클래스는`RCTTVView`이며 tvOS 포커스 엔진을 사용하기위한 추가 메서드가 있습니다. `Touchable` 믹스인에는 포커스 변경을 감지하고 기존 방법을 사용하여 구성 요소의 스타일을 적절하게 지정하고 TV 리모컨을 사용하여보기를 선택할 때 적절한 작업을 시작하는 코드가 추가되어`TouchableWithoutFeedback`,`TouchableHighlight` 및`TouchableOpacity`가 예상대로 작동합니다. 특히:
  - `onFocus`는 터치 가능한 뷰에 초점이 맞춰지면 실행됩니다.
  - `onBlur`는 터치 가능한 뷰의 초점이 맞지 않을 때 실행됩니다.
  - `onPress`는 TV 리모컨의 "선택"버튼을 눌러 실제로 터치 가능한 보기를 선택한 경우 실행됩니다.
- *TV remote/keyboard input*: 새로운 네이티브 클래스`RCTTVRemoteHandler`는 TV 원격 이벤트에 대한 제스처 인식기를 설정합니다. TV 원격 이벤트가 발생하면 이 클래스는`RCTTVNavigationEventEmitter` (`RCTEventEmitter`의 하위 클래스)에 의해 선택되는 알림을 발생시켜 JS 이벤트를 발생시킵니다. 이 이벤트는`TVEventHandler` 자바 스크립트 개체의 인스턴스에 의해 선택됩니다. TV 원격 이벤트의 사용자 지정 처리를 구현해야하는 애플리케이션 코드는 다음 코드에서와 같이`TVEventHandler`의 인스턴스를 만들고 이러한 이벤트를 수신 할 수 있습니다:

```js
var TVEventHandler = require('TVEventHandler');

class Game2048 extends React.Component {
  _tvEventHandler: any;

  _enableTVEventHandler() {
    this._tvEventHandler = new TVEventHandler();
    this._tvEventHandler.enable(this, function (cmp, evt) {
      if (evt && evt.eventType === 'right') {
        cmp.setState({ board: cmp.state.board.move(2) });
      } else if (evt && evt.eventType === 'up') {
        cmp.setState({ board: cmp.state.board.move(1) });
      } else if (evt && evt.eventType === 'left') {
        cmp.setState({ board: cmp.state.board.move(0) });
      } else if (evt && evt.eventType === 'down') {
        cmp.setState({ board: cmp.state.board.move(3) });
      } else if (evt && evt.eventType === 'playPause') {
        cmp.restartGame();
      }
    });
  }

  _disableTVEventHandler() {
    if (this._tvEventHandler) {
      this._tvEventHandler.disable();
      delete this._tvEventHandler;
    }
  }

  componentDidMount() {
    this._enableTVEventHandler();
  }

  componentWillUnmount() {
    this._disableTVEventHandler();
  }
}
```

- *Dev Menu support*: 시뮬레이터에서 cmd-D는 iOS와 유사한 개발자 메뉴를 표시합니다. 실제 Apple TV 장치에서 불러 오려면 리모컨의 재생/일시 중지 버튼을 길게 누릅니다. (작동하지 않는 Apple TV 장치를 흔들지 마세요 :) )
- *TV remote animations*: `RCTTVView` 네이티브 코드는 사용자가 뷰를 탐색 할 때 눈을 안내하는 데 도움이되는 Apple 권장 시차 애니메이션을 구현합니다. 애니메이션은 새로운 선택적 뷰 속성으로 비활성화하거나 조정할 수 있습니다.
- *Back navigation with the TV remote menu button*: 원래 Android 뒤로 버튼을 지원하도록 작성된`BackHandler` 구성 요소는 이제 TV 리모컨의 메뉴 버튼을 사용하여 Apple TV에서 뒤로 탐색을 지원합니다.
- *TabBarIOS behavior*: `TabBarIOS` 구성 요소는 Apple TV에서 다르게 작동하는 기본`UITabBar` API를 래핑합니다. tvOS에서 탭 표시 줄이 불안정하게 다시 렌더링되는 것을 방지하기 위해 ([this issue](https://github.com/facebook/react-native/issues/15081)를 참조하세요.) 선택한 탭 표시 줄 항목은 JavaScript에서만 설정할 수 있습니다. 초기 렌더링시 사용자가 네이티브 코드를 통해 제어합니다.
- *Known issues*:
  - [ListView scrolling](https://github.com/facebook/react-native/issues/12793). 이 문제는 ListView 및 유사한 구성 요소에서`removeClippedSubviews`를 false로 설정하여 해결할 수 있습니다. 이 문제에 대한 자세한 내용은 [this PR](https://github.com/facebook/react-native/pull/12944)을 참조하세요.
