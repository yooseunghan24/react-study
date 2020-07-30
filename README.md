# react-study
리액트 공부 내용 기록.(출처: Do it! 리액트 프로그래밍 정석)
# JSX
- 개발자가 JSX를 작성하면 리액트 엔진은 JSX를 기존 자바스크립트로 해석하는 역할을 함. 이것을 선언형 화면(Declarative View)기술이라 부름. JSX는 개발자가 화면 구성에만 집중할 수 있도록 도와줌.
# 컴포넌트
- 기존의 웹 프레임워크는 MVC 방식으로 정보, 화면, 구동 코드를 분리하여 관리함. 정보 담당을 모델(Model), 화면 담당을 뷰(View), 구동 담당을 컨트롤러(Controller)라고 부르는 것에서 MVC라는 용어가 나옴. 이 방식은 코드 관리를 효율적으로 할 수 있다는 장점이 있으나 MVC 각 요소의 의존성이 높아(하나만 바꾸기가 쉽지 않음) 재활용이 어려움.
- 웹 사이트의 화면은 각 요소가 비슷하고 반복적으로 사용한 경우가 많음. 이 점을 착안하여 컴포넌트가 등장. 컴포넌트는 MVC의 뷰를 독립적으로 구성하여 재사용도 할 수 있고 컴포넌트를 통해 새로운 컴포넌트를 쉽게 만들 수도 있음.
- 컴포넌트 이름의 첫 번째 글자는 반드시 대문자여야 함. 기존의 HTML 마크업과 구분하기 위해서.
### 프로퍼티
- 프로퍼티는 상위 컴포넌트가 하위 컴포넌트에 값을 전달할 때 사용함(단방향으로 데이터가 흐른다). 이때 **프로퍼티 값은 수정할 수 없음** .
- 프로퍼티는 속성(attribute) 형태로 전달됨. 예) \<MyComponent name="message" />
- 프로퍼티에서는 자바스크립트의 자료형을 모두 사용할 수 있음. 이때 프로퍼티의 자료형은 미리 선언해주는 것이 좋음. 그 이유는 리액트 엔진이 프로퍼티로 전달하는 값의 변화를 효율적으로 감지할 수 있고, 개발자가 실수로 지정되지 않은 자료형을 프로퍼티에 전달하려고 할 때 경고 메시지로 알려주기 때문임.
- 문자열 외의 값은 따옴표 대신 중괄호를 사용해야함. 실무에서는 개발자 간의 실수를 줄이기 위해 변수에 객체를 담아 전달하는 방식을 선호함.
- 불리언의 경우 이름만 선언하면 true로 전달되고 이름을 생략하면 false를 전달함.(실무에서 자주 사용함)
### state
- 값을 저장하거나 변경할 수 있는 객체.
- 생성자에서 반드시 초기화해야 함. 그렇지 않으면 내부 함수에서 state값에 접근할 수 없음, 마땅한 값이 없다면 빈 객체라도 넣어야 함(this.state = {};). staste에 저장되는 객체의 값은 직접 변경하면 안 됨.
- state값을 변경할 때는 setState() 함수(상태 관리 함수)를 반드시 사용해야 함. 직접 state값을 변경해도 render() 함수는 새로 호출 되지 않음. setState() 함수를 호출하여 state값을 변경하면 리액트 엔진이 자동으로 render() 함수를 호출함.
- setState() 함수는 비동기로 처리되며, setState() 코드 이후로 연결된 함수들의 실행이 완료된 시점에 화면 동기화 과정을 거침.
- setState() 함수의 인자로 함수를 전달하면 이전 state값을 따로 읽는 과정을 생략할 수 있음.
```
// 일반 함수 사용한 예
example(date) {
  this.setState(function(prevState) {
    const newState = {
      loading: false,
      formData: data + prevState.formData,
    };
    return newState;
  });
}
// 화살표 함수 사용한 예
example(data) {
  this.setState(prevState => {
    loading: false,
    formData: data + prevState.formData,
  });
}
```
- 꼭 setState() 함수로 state를 관리할 필요는 없음. 출력 검증 작업 없이 함수가 호출될 때마다 새롭게 화면을 출력하고 싶다면 클래스 인스턴스 변수와 화면을 강제로 출력해주는 forceUpdate() 함수를 사용하면 됨. (단, 이 방법은 리액트 성능에 제약이 있으므로 매번 새롭게 화면을 출력해야 되는 경우가 아니라면 가급적 사용하지 않는 것을 권장함.)
### 생명주기 함수
#### constructor(props) 함수
- 맨 처음에 생성될 때 한 번만 호출됨.
- 상태(state 또는 객체 변수)를 선언할 때 사용됨.
- 항상 super() 함수를 가장 위에 호출해야 함. super() 함수에 프로퍼티와 생명 주기 상태 등을 초기화하는 과정을 포함하고 있기 때문임.
#### render() 함수
- 데이터가 변경되어 새 화면을 그려야 할 때 자동으로 호출되는 함수.
- render() 함수가 반환하는 JSX를 화면에 그려줌.
#### static getDerivedStateFromProps(props, state) 함수
- getDerivedStateFromProps() 함수는 정적 함수임. 따라서 함수 안에서 this.props나 this.state와 같은 방법으로 프로퍼티나 state값에 접근할 수 없음.
- 각 값에 접근해야 하는 경우 반드시 인자로 전달된 props, state를 이용해야 함. 이때 props는 상위 컴포넌트에서 전달된 값이며, state는 현재 컴포넌트의 state값임.
- 이 함수는 상위 컴포넌트에서 전달받은 프로퍼티로 state값을 연동할 때 주로 사용되며, 반환값으로 state를 변경함.
#### componentDidMount() 함수
- render() 함수가 JSX를 화면에 그린 이후에 호출되는 함수.
- 만약 컴포넌트가 화면에 모두 표현된 이후 해야 하는 작업들은 여기에서 하면 됨.
#### shouldComponentUpdate(nextProps, nextState) 함수
- 프로퍼티를 변경하거나 setState() 함수를 호출하여 state값을 변경하면 화면을 새로 출력해야 하는지 판단하는 함수임. 리액트 성능에 영향을 많이 줌.
- 화면 변경을 위해 검증 작업을 해야 하는 경우 이 함수를 사용하면 됨.
- forceUpdate() 함수를 호출하여 화면을 출력하면 이 함수는 호출되지 않음.
#### getSnapshotBeforeUpdate(prevProps, prevState) 함수
- 컴포넌트의 변경된 내용이 가상화면에 완성된 이후 호출되는 함수.
- 이 함수는 컴포넌트가 화면에 실제로 출력되기 전에 호출되므로 화면에 출력될 엘리먼트의 크기 또는 스크롤 위치 등의 DOM 정보에 접근할 때 사용됨.
#### componentDidUpdate(prevProps, prevState, snapshot) 함수
- 컴포넌트가 실제 화면에 출력된 이후 호출되는 함수.
- 이 함수는 부모 컴포넌트로부터 전달된 이전 프로퍼티(prevProps)와 이전 state값(prevState)과 함께 getSnapshotBeforeUpdate() 함수에서 반환된 값(snapshot)을 인자로 전달 받음.
- 이 값들을 이용하여 스크롤 위치를 옮기거나 커서를 이동시키는 등의 DOM 정보를 변경할 때 사용됨.
#### componentWillUnmount() 함수
- 컴포넌트가 소멸되기 직전에 호출되는 함수.
- 컴포넌트에서 감시하고 있는 작업들을 해제할 때 필요한 함수.
- 예를 들어 컴포넌트에 setInterval() 함수가 사용되었다면 이 함수에서 clearInterval() 함수로 해제해야 함. 이러한 해제 작업이 생략되면 메모리 누수 현상이 발생하여 웹 브라우저의 작동이 멈추기도 함.
### PureComponent
- PureComponent 클래스는 Component 클래스를 상속받은 클래스.
- PureComponent 클래스는 shouldComponentUpdate() 함수를 '얕은 비교'를 하도록 재정의 함. 즉, PureComponent 클래스로 만들어진 컴포넌트는 '얕은 비교를 통해 데이터가 변경된 경우'에만 render() 함수 호출함. 반면 Component 클래스로 만들어진 컴포넌트는 항상 render() 함수를 호출함.
### 함수형 컴포넌트
```
function example(props) {
  const {onButtonClick, hasPlan} = props;
  return (
    <div></div>
  );
}
// 클래스형 컴포넌트
class example extends React.Component {
  render() {
    const {onButtonClick, hasPlan} = this.props;
    return (
      <div></div>
    );
  }
}
```
# 웹팩 코드 검색 확장자(webpack module resolution)의 파일 검색 순서
1. 파일 이름에 확장자가 있는 파일을 먼저 임포트함.
2. 파일 이름에 확장자가 없는 경우 웹팩의 확장자 옵션에 정의된 확장자 목록을 보고 해당 확장자 이름을 포함한 파일이 있는지 확인하여 임포트함. 예를 들어 import 'MyFile';의 경우 MyFile.js > MyFile.jsx 순서로 파일을 확인하여 임포트함.
3. 지정 경로에 해당 파일이 없으면 같은 이름의 폴더를 검색함. 같은 이름의 폴더가 있다면 그 안에 있는 index 파일을 검색. 예를 들어 import 'MyComponent';의 경우 MyComponent.js > MyComponent.jsx 순서로 확인함. 파일이 없으면 MyComponent 폴더를 검색하고 해당 폴더가 있는 경우 폴더 안에서 index.js > index.jsx 순서로 파일을 확인하여 임포트함.
# 자잘한 팁
- 쓸데없는 div 처리 방법: \<React.fragment></React.fragment>. 만약 바벨이 지원한다면 \<></> 이렇게 써도 됨.
