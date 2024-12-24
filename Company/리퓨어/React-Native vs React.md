
[정의]
- - React-Native
	-오픈소스 프레임워크
	-React의 방식으로 네이티브 앱을 개발할 수 있는 페이스북의 오픈소스 프레임워크

- React
	  - 프론트엔드
	  - JavaScript 라이브러리

[동작]
-  React-Native
	-코드 실행 > Bridge 이용 > ios, android 각각의 네이티브 언어에 접근

- React
	-Dom 생성 > Virtual Dom 으로 변화된 곳을 캐치해 변화된 Dom으로 변경
```
왜 가상의 돔을 사용하지??
	복잡한 SPA(Single Page Application)에서는 DOM조작이 많이 발생함.
	==  변화를 적용하기 위해 브라우저가 많이 연산해야함
	==   전체적인 프로세스를 비효율적으로 만듦

이때, Vitual DOM 이 있으면 
	 만약에 뷰에 변화가 있으면, 그 변화는 가상DOM에 먼저 적용시키고, 최종적인 결과만 실제 DOM에 전달할 수 있음.
이로써, 브라우저 내에 발생하는 연산의 양을 줄이면서 성능이 개선됨.

```


![[Pasted image 20241119092136.png]]

[차이점]
- 화면 출력
	-React             > React Dom
	-React-Native > AppRegistry

- 문법
	-React             > JSX, HTML 
	-React-Native > JSX, FlexBox  (기존의 css , html 두 개 다 지원 X)


```
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


```
import { AppRegistry } from 'react-native';
import App from './App';
import { name as appName } from './app.json';
import {View, Text} from 'react-native';
import {View , StyleSheet} from 'react-native';

AppRegistry.registerComponent(appName, () => App);
 
class HelloMessage extends React.Component {
  render() {
    return (
      <View>
        <Text>React-Native   Hello React-Native!</Text>
      </View>
    );
  }
}


class Card extends Component {
 render(){
      return (
        <View style={styles.Card}>
          <View style={styles.Card_title} />
          <View style={styles.Card_details} />
        </View>
    )}
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
  }
})

export default Card;

```



### Virtual DOM 이 필요한 이유

```근데 사실 Virtual DOM은 새로운것이 아님.
걍 DOM차원에서는 더블 버퍼링이랑 다름없음.

변화가 일어나면 그걸 오프라인 DOM 트리에 적용시키는거임.
이 DOM트리는 렌더링도 되지 않기 때문에 연산비용이 적음
연산이 끝나고나면, 최종적인 변화를 실제 DOM으로 한번에 던져주는거임. 모든 변화를 하나로 묶어서.
뭐,,레이아웃 계산, 리렌더링의 '규모'는 커지지만, '횟수'는 줄여짐.

그래서 이건 virtual DOM이 없어도 이뤄질 수 있음.
변화가 일어났을때, 변화를 묶어서 DOM fragment에 적용한 다음 기존 DOM에 던져주면 됨.
```
`>>` DOM fragment를 수동으로 하나씩 관리하기보다는 [자동화+추상화] 할게.
 ` 컴포넌트가 DOM조작 요청을 할 때 다른 컴포넌트들과 상호작용하지 않아도 되고, 특정 DOM을 조작할 것이라던지, 이미 조작했다던지에 대한 정보 공유가 필요가 없어짐.`


#### `+`브라우저 workFlow
![[Pasted image 20241119093513.png]]

DOM Tree 생성
	브라우저가 HTML 전달 > 브라우저의 렌더엔진이 파싱 > DOM node 로 이뤄진 트리 생성
	`각 노드는 각 HTML 엘리먼트들과 연관되어있음.`

Render Tree 생성
	 파싱_외부 CSS 파일 + 각 엘리먼트의 inline스타일 > 렌더트리 생성 `==DOM 트리에 새로운 트리`

 `+` render tree behind
	  -렌더트리 만드는 과정에선, 각 요소들의 스타일이 계산됨.
	  + 다른 요소들의 스타일 속성 참조
 ```
  - Webkit에서는 노드의 스타일을 처리 방식(과정) == attachment 라고불림.
  - DOM 트리의 모든 노드들은 'attach'메소드가 있고, 이 메소드는 스타일 정보를 계산해서 객체형태로 반환함.
  - 이 모든 과정은 동기적(Synchronous)작엄 == DOM트리에 새로운 노드가 추가되면 그 노드의 attach메소드가 실행됨.
  - 
```

Layout (= reflow)
	-레이아웃 
	  `각 노드들은 스크린의 좌표가 주어지고, 정확히 어디에 나타나야 할 지 위치가 주어짐`

painting
	-렌더링된 요소들에 색 입힘 == 스크린에 원하는 정보 나타남.




## `+` React != fast 

![[Pasted image 20241119102232.png]]
( 미신 : React가 DOM보다 빠르다
  사실 : 이거는 유지가능한 어플리케이션을 도와주는거고, 대부분 사용에서 '충분히 빠른거'임 )








참조 : https://velopert.com/3236 
참조 : https://web.dev/articles/howbrowserswork?hl=ko