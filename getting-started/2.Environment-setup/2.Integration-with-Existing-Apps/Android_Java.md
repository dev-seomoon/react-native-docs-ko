# 기존 앱과의 통합

새 모바일 앱을 처음부터 시작할 때 Ract Native가 좋습니다. 그러나 기존 native 어플리케이션에 single view 또는 사용자 흐름을 추가하는 데도 유용합니다. 몇 단계로 새로운 React Native 기반 features, screens, view 등을 추가할 수 있습니다.

구체적인 단계는 대상 플랫폼에 따라 다릅니다.

- **Android (Java)**

- iOS (Objective-C)
- iOS (Swift)

## 핵심 개념

Ract Native 구성 요소를 Android 애플리케이션에 통합하는 방법은 다음과 같습니다.

1. React Native 종속성 및 디렉토리 구조를 설정합니다.
2. JavaScript에서 React Native 구성 요소를 개발하십시오.
3. Android 앱에 'RactRootView'를 추가합니다. 이 보기는 React Native 구성 요소의 컨테이너 역할을 합니다.
4. React Native 서버를 시작하고 기본 애플리케이션을 실행합니다.
5. 애플리케이션의 React Native aspect가 예상대로 작동하는지 확인합니다.

## 전제 조건

[환경 설정 가이드](https://reactnative.dev/docs/environment-setup) 의 React Native CLI Quickstart에 따라 Android용 React Native 앱을 구축하기 위한 개발 환경을 구성합니다.

### 1. 디렉터리 구조 설정하기

원활한 환경을 위해 통합 React Native 프로젝트의 새 폴더를 생성한 다음 기존 Android 프로젝트를 `/android` 하위 폴더에 복사하십시오.

### 2. JavaScript 종속성 설치

프로젝트의 루트 디렉터리로 이동한 후 다음 내용으로 새 `package.json` 파일을 만드십시오.

```jsx
{
"name": "MyReactNativeApp",
"version": "0.0.1",
"private": true,
"scripts": {
    "start":  "yarn react-native start"
    }
}
```

다음으로, [yarn package manager 설치](https://yarnpkg.com/lang/en/docs/install/) 를 확인하세요.

`react` 와 `react-native` 패키지를 설치합니다. 터미널 또는 명령 프롬프트를 연 다음, `package.json` 파일이 있는 디렉토리로 이동하여 실행하세요:

```jsx
$  yarn  add  react-native
```

이렇게 하면 다음과 유사한 메시지가 출력됩니다(스크롤하여 출력을 확인합니다).

> warning "react-native@0.52.2" has unmet peer dependency "react@16.2.0".

이제 React를 설치해야합니다.:

```jsx
$  yarn  add  react@version_printed_above
```

Yarn이 새 `/node_modules` 폴더를 만들었습니다. 이 폴더는 프로젝트를 빌드하는 데 필요한 모든 JavaScript 종속성을 저장합니다.

`node_modules/` 를 `.gitignore` 파일에 추가하세요.

## 앱에 React Native 추가하기

### maven 설정하기

앱의 `build.gradle` 파일에 React Native 및 JSC 종속성을 추가합니다.

```jsx
dependencies {

implementation  "com.android.support:appcompat-v7:27.1.1"

...

implementation  "com.facebook.react:react-native:+"  // From node_modules

implementation  "org.webkit:android-jsc:+"

}
```

> 기본 빌드에서 항상 특정 React Native 버전을 사용하고 있는지 확인하려면 +를 npm에서 다운로드한 실제 React Native 버전으로 바꿉니다.

로컬 React Native 및 JSC maven 디렉토리에 대한 항목을 최상위 수준 `build.gradle`에 추가합니다. 다른 maven 리포지토리 위의 "allprojects" 블록에 추가해야 합니다.

```jsx

allprojects {

repositories {

maven {

// All of React Native (JS, Android binaries) is installed from npm

url  "$rootDir/../node_modules/react-native/android"

}

maven {

// Android JSC is installed from npm

url("$rootDir/../node_modules/jsc-android/dist")

}

...

}

...

}

```

> 경로가 올바른지 확인하십시오! Android Studio에서 Gradle 동기화를 실행한 후 "Failed to resolve: com.facebook.react:react-native:0.x.x" 오류가 발생하면 안 됩니다.

### 네이티브 모듈 autolinking 활성화하기

[autolinking](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md)을 사용하려면 몇 군데 적용해야 합니다. 먼저 다음 항목을 `settings.gradle`에 추가하십시오.:

```jsx
apply  from: file("../node_modules/@react-native-community/cli-platform-android/native_modules.gradle"); applyNativeModulesSettingsGradle(settings)
```

다음으로 `app/build.gradle` 의 맨 아래에 다음 항목을 추가합니다.:

```jsx
apply  from: file("../../node_modules/@react-native-community/cli-platform-android/native_modules.gradle"); applyNativeModulesAppBuildGradle(project)
```

### 권한 설정하기

다음으로 `AndroidManifest.xml`에 인터넷 사용 권한이 있는지 확인합니다.:

```jsx
<uses-permission android:name="android.permission.INTERNET" />
```

`DevSettingsActivity` 에 접근해야 한다면 `AndroidManifest.xml`에 다음을 추가하세요:

```jsx
<activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
```

이 기능은 개발 서버에서 JavaScript를 다시 로드할 때만 개발 모드에서 사용되므로 필요할 경우 릴리즈 빌드에서 이 기능을 제거할 수 있습니다.

### 클리어텍스트 트래픽 (API level 28+)

> Android 9(API 레벨 28)부터는 기본적으로 일반 텍스트 트래픽이 사용되지 않도록 설정되므로 응용 프로그램이 Metro 번들러에 연결할 수 없습니다. 아래 변경 사항을 통해 디버그 빌드에서 텍스트 트래픽을 지울 수 있습니다.

### 1. 디버그 `AndroidManifest.xml`에 `usesCleartextTraffic` 옵션 적용하기

```jsx
<!-- ... -->
<application
android:usesCleartextTraffic="true"  tools:targetApi="28"  >
<!-- ... -->
</application>
<!-- ... -->
```

이는 릴리즈 빌드에 필요하지 않습니다.

네트워크 보안 구성 및 일반 텍스트 트래픽 정책에 대해 자세히 알아보려면 [다음과 같이 하십시오](https://developer.android.com/training/articles/security-config#CleartextTrafficPermitted).

### 코드 통합

이제 React Native를 통합하기 위해 기본 Android 애플리케이션을 수정하겠습니다.

### The React Native 컴포넌트

첫 번째 코드는 애플리케이션에 통합될 새로운 "High Score" 화면의 실제 리액트 네이티브 코드입니다.

### 1. `index.js` 파일 생성하기

먼저 React Native 프로젝트의 루트에 빈 `index.js` 파일을 생성합니다.

`index.js`는 React Native 애플리케이션의 시작점이며 항상 필요합니다. 이 파일은 React Native 구성 요소 또는 애플리케이션의 일부인 다른 파일을 `require`로 하는 작은 파일일 수도 있고, 필요한 모든 코드를 포함할 수도 있습니다. 우리의 경우, 우리는 모든 것을 `index.js`에 넣을 것입니다.

### 2. React Native 코드 추가하기

`index.js`에서 구성 요소를 생성합니다. 여기 샘플에서는 스타일 `<View>`내에`<Text>` 구성요소를 추가할 것입니다.:

```jsx
import React from "react";

import { AppRegistry, StyleSheet, Text, View } from "react-native";

class HelloWorld extends React.Component {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.hello}>Hello, World</Text>
      </View>
    );
  }
}

var styles = StyleSheet.create({
  container: {
    flex: 1,

    justifyContent: "center",
  },

  hello: {
    fontSize: 20,

    textAlign: "center",

    margin: 10,
  },
});

AppRegistry.registerComponent(
  "MyReactNativeApp",

  () => HelloWorld
);
```

### 3. 개발 오류 오버레이에 대한 권한 설정하기

앱이 Android `API level 23` 이상을 대상으로 하는 경우 개발 빌드에 대해`android.permission.SYSTEM_ALERT_WINDOW` 사용 권한을 가지고 있는지 확인합니다. `Settings.canDrawOverlays(this);`에서 이를 확인할 수 있습니다. 이 오류는 다른 모든 창보다 위에 표시되어야 하므로 개발 빌드에 필요합니다. API 레벨 23(Android M)에 도입된 새로운 권한 시스템으로 인해 사용자가 승인해야 합니다. 다음 코드를 `onCreate()` 메소드의 액티비티에 추가하면 됩니다.

```jsx
private  final  int  OVERLAY_PERMISSION_REQ_CODE = 1; // Choose any value...

if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {

if (!Settings.canDrawOverlays(this)) {

Intent  intent = new  Intent(Settings.ACTION_MANAGE_OVERLAY_PERMISSION,

Uri.parse("package:" + getPackageName()));

startActivityForResult(intent, OVERLAY_PERMISSION_REQ_CODE);

}

}
```

마지막으로, 일관된 UX에 대한 허용 또는 거부된 사례를 처리하려면 `onActivityResult()` 메서드(아래 코드 참조)를 재정의해야 합니다. 또한 `startActivityForResult`를 사용하는 Native Module의 통합을 위해서는 `ReactInstanceManager` 인스턴스의 `onActivityResult` 메서드로 결과를 전달해야 합니다.

```jsx

@Override

protected  void  onActivityResult(int  requestCode, int  resultCode, Intent  data) {

if (requestCode == OVERLAY_PERMISSION_REQ_CODE) {

if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {

if (!Settings.canDrawOverlays(this)) {

// SYSTEM_ALERT_WINDOW permission not granted

}

}

}

mReactInstanceManager.onActivityResult( this, requestCode, resultCode, data );

}

```

### `ReactRootView`의 마법

React Native 런타임을 시작하고 JS 구성 요소를 렌더링하기 위해 네이티브 코드를 추가해 보겠습니다. 이를 위해 `ReactRootView`를 만들어 그 안에 있는 React 애플리케이션을 시작하고 이를 주요 콘텐츠 보기로 설정하는 `Activity`를 만들 예정입니다.

> Android 버전이 <5 인 경우에서는 Activity 대신 com.android.support:appcompat 패키지의 AppCompatActivity를 사용하세요.

```jsx

public  class  MyReactActivity  extends  Activity  implements  DefaultHardwareBackBtnHandler {

private  ReactRootView  mReactRootView;

private  ReactInstanceManager  mReactInstanceManager;



@Override

protected  void  onCreate(Bundle  savedInstanceState) {

super.onCreate(savedInstanceState);

SoLoader.init(this, false);



mReactRootView = new  ReactRootView(this);

List<ReactPackage> packages = new  PackageList(getApplication()).getPackages();

// Packages that cannot be autolinked yet can be added manually here, for example:

// packages.add(new MyReactNativePackage());

// Remember to include them in `settings.gradle` and `app/build.gradle` too.



mReactInstanceManager = ReactInstanceManager.builder()

.setApplication(getApplication())

.setCurrentActivity(this)

.setBundleAssetName("index.android.bundle")

.setJSMainModulePath("index")

.addPackages(packages)

.setUseDeveloperSupport(BuildConfig.DEBUG)

.setInitialLifecycleState(LifecycleState.RESUMED)

.build();

// The string here (e.g. "MyReactNativeApp") has to match

// the string in AppRegistry.registerComponent() in index.js

mReactRootView.startReactApplication(mReactInstanceManager, "MyReactNativeApp", null);



setContentView(mReactRootView);

}



@Override

public  void  invokeDefaultOnBackPressed() {

super.onBackPressed();

}

}

```

> React Native에 스타터 키트를 사용하는 경우 "HelloWorld" 문자열을 index.js 파일의 문자열로 바꿉니다(AppRegistry.registerComponent() 메서드에 대한 첫 번째 인수입니다).

“Sync Project files with Gradle”를 실행합니다.

Android Studio를 사용하는 경우`Alt + Enter`를 사용하여 MyReactActivity 클래스에 누락된 모든 import를 추가합니다. `facebook`패키지의 `BuildConfig`가 아닌 당신의 `BuildConfig`를 사용하십시오.

일부 리액트 네이티브 UI 구성 요소가 이 theme를 사용하므로`MyReactActivity`이라는 theme를 `Theme.AppCompat.Light.NoActionBar`로 정해야 합니다.

```jsx
<activity
  android:name=".MyReactActivity"
  android:label="@string/app_name"
  android:theme="@style/Theme.AppCompat.Light.NoActionBar"
></activity>
```

> ReactInstanceManager는 여러 작업 및/또는fragments로 공유할 수 있습니다. ReactFragment 또는 ReactActivity를 직접 만들고 ReactInstanceManager를 보유하는 싱글톤 홀더를 보유하고자 합니다. ReactInstanceManager(예: ReactInstance Manager를 해당 Activities 또는 Fragments의 lifecycle 에 연결)가 필요한 경우 싱글톤에서 제공하는 기능을 사용하십시오.

다음으로, 작업 수명 주기 콜백을 `ReactInstanceManager`와 `ReactRootView`에 전달해야 합니다.:

```jsx

@Override

protected  void  onPause() {

super.onPause();



if (mReactInstanceManager != null) {

mReactInstanceManager.onHostPause(this);

}

}



@Override

protected  void  onResume() {

super.onResume();



if (mReactInstanceManager != null) {

mReactInstanceManager.onHostResume(this, this);

}

}



@Override

protected  void  onDestroy() {

super.onDestroy();



if (mReactInstanceManager != null) {

mReactInstanceManager.onHostDestroy(this);

}

if (mReactRootView != null) {

mReactRootView.unmountReactApplication();

}

}

```

또한 React Native에 back button events를 전달해야 합니다.:

```jsx

@Override

public  void  onBackPressed() {

if (mReactInstanceManager != null) {

mReactInstanceManager.onBackPressed();

} else {

super.onBackPressed();

}

}

```

이를 통해 JavaScript는 사용자가 하드웨어 back button(예: implement navigation)을 누를 때 발생하는 작업을 제어할 수 있습니다. JavaScript에서 뒤로 버튼을 누르지 않으면 `invokeDefaultOnBackPressed` 메서드가 호출됩니다. 기본적으로 `Activity` 가 완료됩니다.

마지막으로 개발 메뉴를 연결해야 합니다. 기본적으로 이 기능은 장치를 흔들 때(rage) 활성화되지만 에뮬레이터에서는 사용하기 어렵습니다. 따라서 하드웨어 메뉴 버튼을 누르면 표시됩니다(Android Studio 에뮬레이터를 사용하는 경우 `Ctrl + M`사용).

```jsx

@Override

public  boolean  onKeyUp(int  keyCode, KeyEvent  event) {

if (keyCode == KeyEvent.KEYCODE_MENU && mReactInstanceManager != null) {

mReactInstanceManager.showDevOptionsDialog();

return  true;

}

return  super.onKeyUp(keyCode, event);

}

```

이제 activity는 JavaScript code를 실행할 준비가 되었습니다.

### 통합 테스트

이제 React Native를 현재 애플리케이션과 통합하기위한 모든 기본 단계를 완료했습니다. 이제 [Metro bundler](https://facebook.github.io/metro/)를 시작하여 `index.bundle` 패키지를 빌드하고, 이를 제공하기 위해 localhost에서 실행중인 서버를 빌드합니다.

### 1. 패키저 실행

앱을 실행하려면 먼저 개발 서버를 시작해야합니다. 이를 위해서, React Native 프로젝트의 루트 디렉터리에서 다음 명령을 실행하세요:

```jsx

$  yarn  start

```

### 2. 앱 실행하기

이제 정상적으로 Android 앱을 빌드하고 실행합니다.

앱 내에서 React 기반 활동에 도달하면 개발 서버에서 JavaScript 코드를 로드하고 다음을 표시해야합니다:

![image](https://user-images.githubusercontent.com/65345381/113375058-94321b80-93a9-11eb-94d9-c352701a6171.png)

### Android Studio에서 릴리스 빌드 생성하기

Android Studio를 사용하여 릴리스 빌드를 만들 수 있습니다! 기존의 native Android 앱의 release build를 만드는 것만큼 빠릅니다. 모든 릴리스 빌드 전에 수행해야하는 추가적인 단계가 하나 있습니다. native Android 앱에 포함될 React Native 번들을 생성하려면 다음을 실행해야합니다:

```jsx

$  npx  react-native  bundle --platform  android --dev  false --entry-file  index.js --bundle-output  android/com/your-company-name/app-package-name/src/main/assets/index.android.bundle --assets-dest  android/com/your-company-name/app-package-name/src/main/res/

```

> 경로를 올바른 경로로 바꾸고, 만약 assets 폴더가 없다면 이를 만드는 것을 잊지 마십시오.

이제 기존과 같이 Android Studio 내에서 native 앱의 릴리스 빌드를 생성하면됩니다.

### 이제 무엇을 할까요?

이 시점에서 기존처럼 앱을 계속 개발할 수 있습니다. React Native 작업에 대한 자세한 내용은 [debugging](https://reactnative.dev/docs/debugging) 및 [deployment](https://reactnative.dev/docs/running-on-device)문서를 참조하세요.
