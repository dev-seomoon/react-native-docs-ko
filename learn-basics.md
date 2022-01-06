# Learn the Basics

React Native는 React와 비슷하지만, 웹 컴포넌트 대신 네이티브 컴포넌트를 빌드 블럭으로 사용합니다. 따라서 React Native 앱의 기본 구조를 이해하려면, JSX, 컴포넌트, `state`, `props` 와 같은 몇 가지 기본 React 개념을 이해해야 합니다. 이미 React를 알고 있더라도, 네이티브 컴포넌트 등 React Native 관련 내용을 학습해야 합니다. 이 튜토리얼은 React 경험 유무에 관계없이 모든 사람을 대상으로 합니다. 

시작해봅시다. 

## Hello World

오래된 전통에 따라 우리는 먼저 "Hello, world!"를 출력하는 것 외에는 아무것도 하지 않는 앱을 만들어야 합니다. 

**Hello World**

```jsx
import React from 'react';
import { Text, View } from 'react-native';

const HelloWorldApp = () => {
  return (
    <View
      style={{
        flex: 1,
        justifyContent: "center",
        alignItems: "center"
      }}>
      <Text>Hello, world!</Text>
    </View>
  )
}
export default HelloWorldApp;
```

궁금하다면 위의 예제 코드를 클릭하여 바로 실행해볼 수 있습니다. 코드를 `App.js` 파일에 붙여넣기해서 로컬 컴퓨터에서 실제 앱을 만들어볼 수도 있습니다. 

## 무슨 일이 일어나고 있는걸까요?

1. 우선 각 플랫폼의 네이티브 컴포넌트로 변환될 `JSX` 를 사용하기 위해 `React` 를 import해야합니다. 
2. 2번째 줄에서는, `react-native`에서 `Text`와 `View` 를 import합니다. 

그런 다음 [함수형 컴포넌트](https://reactjs.org/docs/components-and-props.html#function-and-class-components)이며 웹에서의 React와 같은 방식으로 동작하는 `HelloWorldApp` 함수를 찾습니다. 이 함수는 일부 스타일과 `Text` 를 하위 컴포넌트로 갖는 `View` 컴포넌트를 반환합니다. 

`Text` 컴포넌트를 사용하면 텍스트를 렌더링할 수 있고, `View` 컴포넌트는 컨테이너를 렌더링할 수 있습니다. 이 컨테이너에는 여러 가지 스타일이 적용되어 있습니다. 각 스타일이 어떤 역할을 하고 있는지 분석해 보겠습니다. 

첫 번째 스타일은 `flex: 1`입니다. [`flex`](https://reactnative.dev/docs/layout-props#flex) prop은 주축을 따라 사용 가능한 공간을 어떻게 "채울지"를 정의합니다. 컨테이너가 한 개뿐이기 때문에, 상위 컴포넌트의 사용 가능한 공간을 모두 차지하게 됩니다. 이 경우, 이 컴포넌트가 유일한 컴포넌트이므로, 사용 가능한 모든 화면 공간을 차지하게 됩니다. 

다음 스타일은 [`justifyContent`](https://reactnative.dev/docs/layout-props#justifycontent): "center"입니다. 이렇게 하면 컨테이너의 자식 요소들이 컨테이너 주축의 중앙에 정렬됩니다. 마지막으로, [`alignItems`](https://reactnative.dev/docs/layout-props#alignitems): "center"가 있습니다. 이 스타일은 컨테이너의 자식 요소들을 컨테이너 교차축의 중앙에 정렬됩니다. 

이 중 일부는 JavaScript처럼 보이지 않을 수 있지만, 당황하지 마십시오. 이것이 미래입니다. 

우선, ES2015 (ES6)는 이제는 공식적인 표준의 일부가 된 JavaScript의 개선 버전이지만, 아직 모든 브라우저에서 지원되지는 않습니다. 따라서 웹 개발에서는 아직 사용되지 않는 경우가 많습니다. 그러나 React Native는 ES2015를 지원하므로, 호환성 걱정 없이 ES6를 사용할 수 있습니다. ES2015에 대해 잘 모른다면 본 튜토리얼에서와 같이 샘플 코드를 통해 확인할 수 있습니다. 필요한 경우 [이 페이지](https://babeljs.io/learn-es2015/)에서 ES2015 기능들에 대한 좋은 개요가 제공됩니다. 

이 예제 코드의 또 다른 특이한 부분은 `<View><Text>Hello world!</Text></View>` 입니다. 이것은 JavaScript에 XML을 내장하는 문법인 JSX입니다. 많은 프레임워크는 마크업 언어 안에 코드를 포함시킬 수 있는 특수한 템플릿을 사용합니다. React에서는 반대로, JSX를 통해 코드 안에 마크업 언어를 포함시킬 수 있습니다. JSX는 웹에서의 `<div>`나 `<span>`과 같은 것들 대신에 React 컴포넌트를 사용한다는 점을 제외하면, 웹에서 HTML처럼 작동합니다. 이 경우 `<Text>`는 일부 텍스트를 표시하는 [코어 컴포넌트](https://reactnative.dev/docs/intro-react-native-components)이고, `View`는 `<div>` 또는 `<span>`과 비슷합니다. 

## 컴포넌트

이 코드는 새로운 컴포넌트인 `HelloWorldApp`을 정의하고 있습니다. React Native 앱을 구축할 때는, 새로운 컴포넌트를 많이 만들게 될 것입니다. 화면에 보이는 모든 것이 일종의 컴포넌트입니다. 

## Props

대부분의 컴포넌트는 생성될 때 서로 다른 매개변수를 통해 커스터마이징될 수 있습니다. 이러한 생성 매개변수들을 props라고 합니다. 

사용자가 만든 컴포넌트 또한 `props`를 사용할 수 있습니다. 이렇게 하면 앱의 여러 위치에서 사용되는 하나의 컴포넌트를 만들고, 각 위치에서 조금씩 다른 속성을 가질 수 있습니다. 함수형 컴포넌트에서 `props.YOUR_PROP_NAME`를 참조하거나, 클래스형 컴포넌트에서 `this.props.YOUR_PROP_NAME`을 참조하십시오. 다음은 예시입니다. 

**Hello Props**

```jsx
import React from 'react';
import { Text, View, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  center: {
    alignItems: 'center'
  }
})

const Greeting = (props) => {
  return (
    <View style={styles.center}>
      <Text>Hello {props.name}!</Text>
    </View>
  );
}

const LotsOfGreetings = () => {
  return (
    <View style={[styles.center, {top: 50}]}>
      <Greeting name='Rexxar' />
      <Greeting name='Jaina' />
      <Greeting name='Valeera' />
    </View>
  );
}

export default LotsOfGreetings;
```

`name`을 prop으로 사용해 `Greeting` 컴포넌트를 커스터마이징할 수 있으므로, 환영인사를 할 때마다 이 컴포넌트를 재사용할 수 있습니다. 이 예제는 또한 JSX에서 `Greeting` 컴포넌트를 사용합니다. 이것이 바로 React를 그렇게 멋지게 만드는 힘입니다. 

여기에서 나타나는 또 다른 새로운 점은, [`View`](https://reactnative.dev/docs/view) 컴포넌트입니다. [`View`](https://reactnative.dev/docs/view)는 스타일과 레이아웃을 제어하는 데에 도움이 되며, 다른 컴포넌트들의 컨테이너로서 유용합니다. 

`props`와 기본적인 [`Text`](https://reactnative.dev/docs/text), [`Image`](https://reactnative.dev/docs/image), [`View`](https://reactnative.dev/docs/view) 컴포넌트로 다양한 정적 화면을 구축할 수 있습니다. 시간에 따라 앱이 변화하게 만드는 방법을 알고 싶다면 State에 대해 학습할 필요가 있습니다. 

## State

[읽기 전용](https://reactjs.org/docs/components-and-props.html#props-are-read-only)이므로 수정해서는 안 되는 props와 달리, `state`는 사용자의 행동, 네트워크 응답 등에 따라 시간이 지남에 따라 React 컴포넌트가 출력을 변경할 수 있습니다. 

#### React에서 state와 props의 차이는 무엇일까요?

React 컴포넌트에서 props는 부모 컴포넌트로부터 자식 컴포넌트로 전달되는 변수입니다. 마찬가지로 state 또한 변수입니다. 그러나 state는 매개변수로 전달되지 않으며, 컴포넌트에서 내부적으로 초기화하고 관리합니다. 

#### React와 React Native에서 state를 처리하는 데 차이가 있을까요? 

```jsx
// ReactJS Counter Example using Hooks!

import React, { useState } from 'react';



const App = () => {
  const [count, setCount] = useState(0);

  return (
    <div className="container">
      <p>You clicked {count} times</p>
      <button
        onClick={() => setCount(count + 1)}>
        Click me!
      </button>
    </div>
  );
};


// CSS
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}

```

```jsx
// React Native Counter Example using Hooks!

import React, { useState } from 'react';
import { View, Text, Button, StyleSheet } from 'react-native';

const App = () => {
  const [count, setCount] = useState(0);

  return (
    <View style={styles.container}>
      <Text>You clicked {count} times</Text>
      <Button
        onPress={() => setCount(count + 1)}
        title="Click me!"
      />
    </View>
  );
};

// React Native Styles
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  }
});
```

As shown above, there is no difference in handling the `state` between [React](https://reactjs.org/docs/state-and-lifecycle.html) and `React Native`. You can use the state of your components both in classes and in functional components using [hooks](https://reactjs.org/docs/hooks-intro.html)!

In the following example we will show the same above counter example using classes.

위와 같이 `state`를 처리하는 데 있어서 [React](https://reactjs.org/docs/state-and-lifecycle.html)와 `React Native` 간에 차이는 없습니다. 컴포넌트의 state를 클래스형 컴포넌트에서 사용할 수 있고, [훅(hooks)](https://reactjs.org/docs/hooks-intro.html)을 사용해 함수형 컴포넌트에서도 사용할 수 있습니다. 

다음 예제에서는 함수 대신 클래스를 사용해 구현한 동일한 컴포넌트를 보여줍니다. 

**Hello Classes**

```jsx
import React, { Component } from 'react'
import {
  StyleSheet,
  TouchableOpacity,
  Text,
  View,
} from 'react-native'

class App extends Component {
  state = {
    count: 0
  }

  onPress = () => {
    this.setState({
      count: this.state.count + 1
    })
  }

 render() {
    return (
      <View style={styles.container}>
        <TouchableOpacity
         style={styles.button}
         onPress={this.onPress}
        >
         <Text>Click me</Text>
        </TouchableOpacity>
        <View>
          <Text>
            You clicked { this.state.count } times
          </Text>
        </View>
      </View>
    )
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  button: {
    alignItems: 'center',
    backgroundColor: '#DDDDDD',
    padding: 10,
    marginBottom: 10
  }
})

export default App;
```

