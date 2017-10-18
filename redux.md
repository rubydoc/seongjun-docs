Redux는 Flux에서 영감을 받아서 만들어진 아키텍쳐입니다.(프레임워크가 아닙니다.) 

### 왜 만들어졌나(어떤 문제를 해결하나)
현재 SPA는 많은 상태를 JS코드로 관리합니다.(Stateful Service). 상태와 비동기가 만나는 순간 상태 변경을 추적하기가 너무 어려워집니다.(동시성 문제의 어려움을 떠올리면 될 것 같습니다.). Redux는 EventSourcing을 통해 상태 변경을 추적하기 쉽게 만들면서 이러한 관리 및 디버깅이 편하도록 합니다. 

### 장단점
#### 장점
  - HMR(Hot Module Replacement)을 사용해서 데이터를 보존하면서 소스코드 변경을 바로 반영할 수 있습니다. (BrowserSync보다 개선된 방법)
  - 시간 여행자를 통해 데이터를 볼 수 있습니다.
  - 상태가 많은 SPA를 구현할 때 좋습니다.
  - 단일 데이터를 변경시키는 모든 액션을 한꺼번에 보기 좋습니다.

#### 단점
  - 여러가지 조합에서 오는 문법적 혼동이 있습니다. (ECMA6, Reselect, Redux, JSX, React)
  - 반복적인 타이핑이 있습니다.
  - 상태가 별로 없는 단순 CRUD에서는 오히려 복잡해집니다.

### 구조
- Store
  - 데이터를 저장합니다.
  - 단 하나의 Root Store를 가지고 있습니다.
- Action
  - Reducer에서 조합될 데이터를 호출하고 보냅니다.
- Reducer
  - Action을 통해 받은 데이터로 state를 변경합니다.
  - Reducer는 순수하게 유지해야합니다, 순수함을 유지한다는 것은 API 호출 또는 인수 변경 등을 하지 않고 단순 계산만 해야한다는 의미입니다.
- View
  - Store의 상태와 데이터를 이용해서 Rendering합니다. Action을 UI 이벤트에 붙여 Action을 실행하도록 합니다.

### 단방향 데이터 흐름
![1](./redux/1)
1. Action을 통해 Store에 데이터 변경을 알립니다.
2. Reducer가 인수를 받아서 계산해서 실제 Store의 데이터를 변경합니다.(CopyOnWrite)
3. Store의 변경이 감지되면 View는 반응적으로 데이터를 표현합니다.
4. View에서 일어나는 Click 또는 입력에 의해서 Action을 호출합니다.

### 시간 여행(Redux DevTools)
[Redux Devtools 다운로드 ](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=ko)
![2](./redux/2)

### Q&A
  - API는 어디에서 호출해야하나요?
    - Action에서 Store에 전달할 새로운 데이터를 결정하기 때문에 Action에서 호출해야합니다.
  - 첫 상태는 어디서 정하나요?
    - 상태는 Reducer에 의해서 정해지므로 Reducer에서 결정합니다.
  - 왜 Action과 Reducer를 나누나요?
    - Reducer는 순수한 상태로 유지해야하기 때문입니다.

### 같이 알면 좋은 것들
  - EventSourcing
    - 데이터의 마지막 상태를 저장하는 것이 아니라 이벤트의 합(스냅샷)으로 데이터의 현재 상태를 나타냅니다, 데이터 변화 추이를 볼 수 있습니다.
  - 동시성을 관리하는 현대적 방법들
    - Immutable
      - 불변하는 객체는 언제 어디서 참조하든지 참조 무결성합니다.
    - CopyOnWrite
      - 동시성 문제의 해법으로 변경이 생길 때마다 모든 쓰레드에 해당 객체를 복사합니다. 
