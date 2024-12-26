
# React와 React-Native의 비교 및 Virtual DOM의 역할

## 소개
React와 React-Native는 각각 웹과 네이티브 애플리케이션 개발을 위한 도구로, 서로 다른 방식으로 동작하지만 유사한 원칙을 따름.                                  
React와 React-Native의 정의, 동작 원리, 차이점을 정리하고, Virtual DOM의 역할과 필요성에 대해 설명함.

---

## 주요 학습 내용

### 1. 정의
#### React-Native
- 오픈소스 프레임워크.
- React의 방식으로 네이티브 앱(iOS, Android)을 개발할 수 있음.
- 페이스북에서 개발.

#### React
- 프론트엔드 JavaScript 라이브러리.
- 웹 애플리케이션 개발에 특화.

---

### 2. 동작 원리
#### React-Native
- 코드 실행 후 **Bridge**를 이용하여 iOS와 Android의 네이티브 언어에 접근.

#### React
- DOM을 생성한 후 **Virtual DOM**을 통해 변경된 부분만 실제 DOM으로 반영.

---

### 3. Virtual DOM의 필요성
#### 배경
- 복잡한 SPA(Single Page Application)에서는 DOM 조작이 빈번히 발생하며, 이는 브라우저의 연산량 증가로 이어짐.
- 이러한 문제를 해결하기 위해 Virtual DOM을 사용.

#### 역할
1. **변화 관리**:
   - 변화가 발생하면 Virtual DOM에 먼저 적용.
   - 최종 결과만 실제 DOM에 반영.
2. **성능 개선**:
   - 브라우저 내 연산량 감소.
   - 렌더링 효율성 향상.

#### Virtual DOM의 원리
- Virtual DOM은 새로운 개념이 아니며, DOM 차원에서는 **더블 버퍼링**과 유사함.
- 변화가 일어나면 오프라인 DOM 트리에 적용 후 최종 결과를 묶어서 실제 DOM에 반영.

#### 장점
- DOM fragment를 수동으로 관리하지 않아도 됨.
- 컴포넌트 간 상호작용이 줄어듦.

---

### 4. React와 React-Native의 차이점
| 항목       | React                | React-Native            |
|------------|----------------------|--------------------------|
| 화면 출력  | ReactDOM             | AppRegistry             |
| 문법       | JSX, HTML 지원      | JSX, FlexBox 지원       |

#### React 예제 코드
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));

class HelloMessage extends React.Component {
  render() {
    return (
      <div>
        <h1>React</h1>
        Hello React!
      </div>
    );
  }
}
```

#### React-Native 예제 코드
```javascript
import { AppRegistry } from 'react-native';
import App from './App';
import { name as appName } from './app.json';
import { View, Text, StyleSheet } from 'react-native';

AppRegistry.registerComponent(appName, () => App);

class HelloMessage extends React.Component {
  render() {
    return (
      <View>
        <Text>Hello React-Native!</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  Card: {
    flex: 1,
  },
  Card_title: {
    flex: 0.5,
    backgroundColor: 'blue',
  },
  Card_details: {
    flex: 8,
    backgroundColor: 'green',
  },
});

export default Card;
```

---

### 5. 브라우저 Workflow
#### DOM 생성
- HTML을 브라우저 렌더 엔진이 파싱하여 DOM Tree를 생성.
- 각 노드는 HTML 엘리먼트와 연결.

#### Render Tree 생성
- DOM Tree와 외부 CSS 파일, inline 스타일을 결합하여 Render Tree 생성.

#### Layout (Reflow)
- Render Tree의 각 노드에 좌표를 부여하여 화면에 배치.

#### Painting
- 화면에 요소들을 렌더링하고 색상을 입힘.

---

## 깨달은 것
1. **React와 React-Native의 차이**:
   - React는 DOM을 사용하여 웹을 개발하고, React-Native는 Bridge를 통해 네이티브 언어에 접근함.

2. **브라우저 Workflow 이해**:
   - DOM, Render Tree, Layout, Painting의 과정을 학습하며, 성능 최적화의 중요성을 깨달음.


----

참조 : https://velopert.com/3236 
참조 : https://web.dev/articles/howbrowserswork?hl=ko
