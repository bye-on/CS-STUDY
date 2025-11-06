# MVP 패턴

![MVPPattern.png](./image/MVPPattern.png)

뷰와 프레젠터는 일대일 관계이기 때문에 MVC 패턴보다 더 강한 결합을 지닌 디자인 패턴이라고 볼 수 있다. 

⇒ Model과 View는 서로 모른다는 점에서 MVC와는 차이가 있다. 

- Model은 데이터를 보관하고, 외부에서 set을 할 때 이벤트를 날리게 되며 이를 Presenter가 구독한다.
- Presenter는 Model과 View를 참조한다. View에서 들어온 사용자의 입력을 여기서 처리한다. 이벤트에는 Model의 값을 조작하는 로직이 들어가면 된다. 더불어 Model의 값이 변하는 이벤트를 View에 반영하도록 구독하여 둘을 중재한다. 일반적으로 View와 1:1 관계이다.

⇒ Unity, Android에서 사용 

## 왜 MVP를 사용할까?

MVC 패턴에서 비대해지는 Controller의 역할을 줄여주어 문제를 해결하고자 함.

## MVP의 장점

- UI 로직과 비즈니스 로직을 명확하게 분리
- View와 Model 간 결합도가 낮음

## MVP의 단점

- View와 Presenter 간 결합도가 높음 (1:1)
- 프로젝트 규모가 커질수록 관리가 어려움

⇒ UI 로직과 비즈니스 로직을 분리해 View와 Model 간 결합도를 낮추며 MVC의 문제점을 해결했으나, View와 Presenter 간 의존성이 남아있다는 문제가 남아있다.