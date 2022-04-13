# Redux

Redux
-----
context API와 마찬가지로 Cross-Component State와 App-Wide State를 관리하는데 도움을 주는 상태관리 라이브러리.


Context API의 한계
----
1. 애플리케이션 규모가 커질 수록 JSX코드 내부의 깊이가 깊어진다. -> 관리가 힘들어짐.      
2. 인증과 같은 부분은 변경횟수가 적어 문제가 되지 않지만 여러번의 변경횟수가 있다면 Redux에 비해 성능적으로 기대하기 힘듬.

Store, Reducer
----
스토어는 데이터를 관리하는 공간이다. 그리고 스토어에서 사용하는 데이터는 리듀서 함수가 결정한다.    
이유는 리듀서 함수가 새 상태 스냅숏을 보내주기 때문이다. 초기상태에 리듀서는 스토어로 디폴트 값을 보내준다.     
Reducer는 항상 자바스크립트 순수함수여야한다. 즉 외부값에 영향을 주거나 받지 않는다. 또한 사이드 이펙트가 존재하면 안 된다          
순수함수를 사용하는 이유는 항상 예상되는 특정한 객체를 리턴해야하기 때문. (state를 직접 바꾸는 로직이 들어가면 순수함수가 아니다.)     
스토어와 작업하는 건 리듀서기때문에 redux.createStore(countReducer)와 같은 부분이 필히 선행되어야함.


Redux 흐름
-----
1. 유저가 버튼 클릭     
2. 앱은 유저의 행동에 맞는 디스패치를 실행해 액션을 일으킴
3. 스토어는 이전 상태와 현재 액션으로 리듀서 함수를 실행하고, 리턴값을 새로운 상태로 저장
4. 스토어는 스토어를 구독하고 있던 UI들에게 업데이트 되었다고 알려줌.
5. 스토어의 데이터가 필요한 각각의 UI들은 필요한 상태가 업데이트 되었는지 확인.
6. 데이터가 변경된 각 구성요소는 새 데이터로 강제로 다시 렌더링하므로 화면에 표시되는 내용을 업데이트 함.


Redux 세가지 원칙
-----
1. 하나의 애플리케이션 안에는 하나의 스토어만 사용 -> 디버깅이 쉬워지고 서버와의 직렬화가 되어 쉽게 데이터 전달이 가능해짐
2. 상태를 변화시키는 건 오직 액션만. -> 상태를 변화시키려는 의도파악이 가능하며 상태 변경에 대한 추적이 용이해짐.
3. 변화를 일으키는 리듀서 함수는 순수함수.

React 앱에서의 Redux
-----
React 앱 내에서 redux를 사용하는 방법으로 react-redux 패키지가 있다. 이를 사용하는 방법은 사용하고자하는 컴포넌트(자식을 포함하는 혹은 포함하지 않아도 되는)를 Provider로 감싸주는 것이다. Provider는 import를 통해 react-redux에서 가져온 컴포넌트이다. 이후 Provider에 prop으로 store를 설정하면 dispatch, store접근 등 할 수 있게된다.


useSelector
-----
useSelector는 react-redux에서 제공하는 훅이다.     
스토어에는 수많은 데이터들이 있고 프로덕션 레벨에서는 세부적인 데이터에 접근하는게 필요할텐데 이를 쉽게 접근할 수 있는 훅이useSelector이다.     
또한 state가 변경될 때 알아서 트리거를 해주는 역할을 한다. 즉 subscript을 자동으로 해주는 게 useSelector훅이다. 

올바른 Redux state
-----
reducer에서는 항상 새로운 객체를 리턴한다. 즉 원본데이터를 직접 수정하지 않고 새롭게 정의된 객체를 리턴해주는 것이다.     
이렇게 하는 이유는 리덕스 자체에서 새로운 객체를 리턴함으로써 state를 최신의 상태로 만들어주는 로직이 설계되어있기 때문이다. 원본 객체를 직접 변경해서 리턴하는 경우 예측 불가능한 동작이 발생할 수 있고 프로그램을 디버깅하는 것도 어려워질 수 있기 때문이다.     
즉 동기화와 관련해서 state가 정확히 반영되지 않을 수 있기 때문.


Redux를 사용함으로써 생길 수 있는 이슈
----
1. dispatch시 변수 type을 직접 타이핑함으로써 오타가 생길 수 있다. -> 액션이 실행되지 않게 됨. sol) type을 상수로 정의하여 export
2. 애플리케이션의 규모가 커질수록 reducer파일에 많은 로직을 담게 된다. ->  유지보수가 힘들어짐 sol) redux를 나눌 수 있는 라이브러리 사용
3. 항상 새로운 객체를 반환해야하는데 state가 많으면 코드가 길어짐 -> sol) 자동으로 상태를 복사하는 써드파티 라이브러리를 사용

Redux toolkit
-----
Redux toolkit은 위에 소개한 redux의 고질적인 문제들을 쉽게 해결할 수 있기 때문에 강력한 라이브러리이다.    
기본적으로 redux toolkit은 createSlice를 이용해서 slice들을 만들어 이를 configureStore에 넣어줘서 사용한다.     
이는 redux에서 createStore에 reducer함수를 넣는 것과 비슷하다. createSlice의 장점은 action에 따라 state값을 리턴하는 보일러 플레이트 코드를 줄일 수 있는 것(if문으로 action.type확인 제거)과 원본객체를 실제로는 건들지 않으면서 코드는 건들 수 있게 만드는 것이 장점이다. 두번째 장점이 무슨 말이냐면 redux toolkit은 내부적으로 immer패키지를 설치하게 되는데 이는 원본객체에 접근하려는 것을 감지하고 새로운 객체를 생성해서 변경된 부분만 오버라이드해서 리턴하게 된다.     

redux toolkit에서 dispatch를 하는 방법은 counterSlice.actions처럼 뒤에 actions를 호출함으로써 사용할 수 있다. 예시로 counterSlice에 reducer로 정의된 increment리듀서가 있다고 가정해보자. 이를 dispatch하기 위해서는 dispatch({counterSlice.actions.increment()})와 같이 사용하면 접근하여 state를 변경할 수 있다. 이것이 가능한 이유는 actions함수를 실행했을 때 액션 객체를 생성하게 되고 그 액션객체는 내부적으로 식별자를 만들기 때문이다. 이는 오타에 의한 문제를 해결할 수 있고 if문을 통한 type비교를 하지 않아도 되기에 매우 강력하다. 그리고 payload(데이터)를 전달하고 싶다면 호출하는 부분에서는 그냥 data를 넣어주고 받는쪽에서 action.payload로 받으면 된다. 

async, sideEffect에서의 Redux
-----
Redux에서 reducer는 기본적으로 순수함수여야 한다. 이 때문에 sideEffect가 있는 http요청이나 async와 같은 비동기 상황에서는 순수함수 내부에 사용할 수 없게 된다. 이 때문에 우리는 개발할 때 우리의 논리를 어디에 두는가를 결정해야한다. sideeffect나 비동기 상황이 없는 경우에는 순수 reducer를 통해서 개발하고, 만약 이들을 포함한다면 컴포넌트 단에서 처리를 해준뒤 끝처리만 redux에 저장하는 식으로 해야한다.      
하지만 더 좋은 방법이 있는데 이는 useEffect를 컴포넌트단에서 사용하는 것이다. useEffect는 사이드 이펙트 부분을 처리하고 dependency가 변경될 때마다 실행이 되기 때문에 http요청과 같은 부분을 redux와 연결해서 사용할 때 좋은 방법이다. 

Redux toolkit 논리를 어디에 두는가
------
위의 내용에 살을 덧붙여 얘기하자면 논리를 어디에 두느냐에 따라서 사이드 이펙트 코드의 위치가 바뀔 수 있다. 컴포넌트단에서 useEffect를 이용해서 사이드 이펙트를 실행할 수 있지만 컴포넌트의 크기가 커지는 것을 싫어하는 개발자라면 이를 액션 생성자 thunk를 이용해서 해결할 수 있다. 기존의 slice를 정의해둔 파일에서 dispatch시 직접 액션생성자를 만드는 것이다. 이를 위해서 slice외부에 자바스크립트 일반함수를 정의해주고 함수를 리턴하게 만들어준다. 그 함수의 인자로는 dispatch를 받게되고 리턴하는 함수 내부에서 사이드이펙트에 대한 부분을 정의해주면 된다. 이는 리듀서가 순수함수의 형태를 유지할 수 있게 되기 때문에 두번째 대안으로 괜찮은 방법이다.
