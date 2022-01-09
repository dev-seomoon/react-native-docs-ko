# TV기기를 위한 구축

TV 장치 지원은 기존 React Native 응용 프로그램이 Apple TV 및 Android TV에서 작동하도록 구현되었으며 응용 프로그램의 JavaScript 코드를 거의 또는 전혀 변경하지 않아도됩니다.


## 빌드 변경[#](https://reactnative.dev/docs/building-for-tv#build-changes)

- *Native layer* : Android TV에서 React Native 프로젝트를 실행하려면`AndroidManifest.xml`을 다음과 같이 변경해야합니다:


```xml
  <!-- Add custom banner image to display as Android TV launcher icon -->
 <application
  ...
  android:banner="@drawable/tv_banner"
  >
    ...
    <intent-filter>
      ...
      <!-- Needed to properly create a launch intent when running on Android TV -->
      <category android:name="android.intent.category.LEANBACK_LAUNCHER"/>
    </intent-filter>
    ...
  </application>
```

- *JavaScript layer*: Android TV 지원이`Platform.android.js`에 추가되었습니다. 다음을 수행하여 코드가 Android TV에서 실행 중인지 확인할 수 있습니다:

```js
var Platform = require('Platform');
var running_on_android_tv = Platform.isTV;
```



## 코드 변경[#](https://reactnative.dev/docs/building-for-tv#code-changes)

- *Access to touchable controls*: Android TV에서 실행할 때 Android 프레임 워크는 뷰에서 포커스 가능한 요소의 상대적 위치를 기반으로 방향 탐색 체계를 자동으로 적용합니다. `Touchable` 믹스인에는 포커스 변경을 감지하고 기존 방법을 사용하여 구성 요소의 스타일을 적절하게 지정하고 TV 리모컨을 사용하여보기를 선택할 때 적절한 작업을 시작하는 코드가 추가되었으므로`TouchableWithoutFeedback`,`TouchableHighlight`,`TouchableOpacity` 및` TouchableNativeFeedback`이 예상대로 작동합니다. 특히:
  - `onFocus` 는 터치 가능한 뷰에 초점이 맞춰지면 실행됩니다.
  - `onBlur` 는 터치 가능한 뷰가 초점을 벗어나면 실행됩니다.
  - `onPress` 는 TV 리모컨의 "선택"버튼을 눌러 터치 가능한 뷰가 실제로 선택되면 실행됩니다.
- *TV remote/keyboard input*: 새로운 네이티브 클래스`ReactAndroidTVRootViewHelper`는 TV 원격 이벤트에 대한 주요 이벤트 핸들러를 설정합니다. TV 원격 이벤트가 발생하면이 클래스는 JS 이벤트를 발생시킵니다. 이 이벤트는`TVEventHandler` 자바 스크립트 개체의 인스턴스에 의해 선택됩니다. TV 원격 이벤트의 사용자 지정 처리를 구현해야하는 애플리케이션 코드는 다음 코드에서와 같이`TVEventHandler`의 인스턴스를 만들고 이러한 이벤트를 수신 할 수 있습니다:
- 
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

- *Dev Menu support*: 에뮬레이터에서 cmd-M은 Android와 유사한 개발자 메뉴를 표시합니다. 실제 Android TV 기기에서 불러 오려면 메뉴 버튼을 누르거나 리모컨의 빨리 감기 버튼을 길게 누르세요. (안드로이드 TV 기기를 흔들지 마십시오. 작동하지 않습니다. :) )
- *Known issues*:
  - `TextInput` 구성 요소는 현재 작동하지 않습니다. (즉, 자동으로 포커스를받을 수 없습니다. [this comment](https://github.com/facebook/react-native/pull/16500#issuecomment-629285638)를 참조하세요.)
    - 그러나 ref를 사용하여 `inputRef.current.focus()`를 수동으로 트리거 할 수 있습니다.
    - 입력을 `TouchableWithoutFeedback` 구성 요소로 감싸고 해당 터치 가능의 `onFocus` 이벤트에서 포커스를 트리거 할 수 있습니다. 이렇게 하면 화살표 키를 통해 키보드를 열 수 있습니다.
    - 키보드는 키를 누를 때마다 상태를 재설정 할 수 있습니다 (Android TV 에뮬레이터 내에서만 가능할 수 있음).
  - `Modal` 구성 요소의 콘텐츠는 포커스를 받을 수 없습니다. 자세한 내용은 [this issue](https://github.com/facebook/react-native/issues/24448)를 참조하세요.
