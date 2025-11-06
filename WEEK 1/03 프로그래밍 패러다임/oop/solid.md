# 모든 개발자가 알아야 할  SOLID 의 진실 혹은 거짓

---

[모든 개발자가 알아야 할 SOLID의 진실 혹은 거짓](https://tech.kakaobank.com/posts/2411-solid-truth-or-myths-for-developers/)

### SOLID 원칙을 따르면?

> 변경에 유연한 구조
> 
- 변경에 유연하다라는 말은 곧 결합도는 낮고, 응집도 높은 구조를 의미한다.

> 이해하기 쉬운 구조
> 
- 가독성이 좋다.

<aside>
💡

결합도가 낮고 응집도가 높으며 가독성이 좋은 코드를 작성하는 것이 모든 소프트웨어 설계가 추구하는 궁극적인 지향점.

</aside>

### S : SRP (Single Responsibility Principle) 단일책임 원칙

> 모든 클래스는 각각 하나의 책임만 가져야 하는 원칙
> 

? ‘책임’? 이라는 단어는 매우 주관적인 평가

### 단일 모듈은 변경의 이유가 하나, 오직 하나 뿐 이어야 한다.

변경의 이유가 하나라고 정의하게 됨으로써 객관적으로 파악이 가능해졌다.

서로 다른 책임을 분리하면 응집도가 높아지는 효과가 생기고 코드의 가독성과 재사용성이 높아져 유지보수성이 쉬워진다.

### 예시

```jsx
class InterestRate {
    fun getDepositInterestRate(): Float {
        // 수신금리를 계산합니다.
    }

    fun getLoanInterestRate(): Float {
        // 여신금리를 계산합니다.
    }

    private fun getBaseInterestRate(): Float {
        // 기준금리를 계산합니다.
    }
}

잠깐! 여신과 수신이 뭔가요? 🤔
은행업에서 여신과 수신은 은행의 주요 기능 중 일부이며, 금융 시스템 내에서 자금의 순환을 돕는 중요한 역할을 합니다.
- 여신(與信): 은행이 고객에게 자금을 대출하는 활동으로, 이를 통해 은행은 이자 수익을 창출합니다. (예: 대출, 신용카드 발급, 담보대출 등)
- 수신(受信): 고객이 은행에 자금을 예금하는 것을 의미하며, 은행의 운영 자금 및 대출 자금으로 활용됩니다. 예금에 대해서는 이자 지급이 이루어집니다. (예: 예금 계좌, 적금, 예금증서(CD) 등)
```

- 수신금리 계산기, 기준금리를 가져오기 위해 getBasetInterestRate메서드를 동일하게 사용한다고 가정

![image.png](%EB%AA%A8%EB%93%A0%20%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80%20%EC%95%8C%EC%95%84%EC%95%BC%20%ED%95%A0%20SOLID%20%EC%9D%98%20%EC%A7%84%EC%8B%A4%20%ED%98%B9%EC%9D%80%20%EA%B1%B0%EC%A7%93%202a21b16048c280ba8d34c3c476278636/image.png)

- 여신팀에서 기준 금리 계산하는 로직을 수정했다고 가정
- 수신 팀은 모두 정상적으로 기능이 동작했지만 며칠이 지나서야 수신금리가 잘못 계산되는 것을 알게 된다면?

→ 공통 코드를 수정할때 옆팀에서 공유를 잘하자? 라는 원칙보다는 책임이 분리되어야 한다는 이론.

![image.png](%EB%AA%A8%EB%93%A0%20%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80%20%EC%95%8C%EC%95%84%EC%95%BC%20%ED%95%A0%20SOLID%20%EC%9D%98%20%EC%A7%84%EC%8B%A4%20%ED%98%B9%EC%9D%80%20%EA%B1%B0%EC%A7%93%202a21b16048c280ba8d34c3c476278636/image%201.png)

<aside>
💡

결국 이유는 하나의 기능이라고 생각한다.
하나의 클래스에 속한 메서드들은 하나의 공통된 목표를 가지고 동작해야하며 두가지 이상의 목표가 있다면 SRP 원칙에 따라 분리하는 것이 좋다.

</aside>

### O : OCP(Open Closed Principle) 개방 - 폐쇄 원칙

> 유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야하는 원칙
> 

→ “확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다.

![image.png](%EB%AA%A8%EB%93%A0%20%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80%20%EC%95%8C%EC%95%84%EC%95%BC%20%ED%95%A0%20SOLID%20%EC%9D%98%20%EC%A7%84%EC%8B%A4%20%ED%98%B9%EC%9D%80%20%EA%B1%B0%EC%A7%93%202a21b16048c280ba8d34c3c476278636/image%202.png)

- 현재 상화 스크롤만 지원되는 기능에서 좌우  스와이프 기능을 추가로 개발한다면?

![image.png](%EB%AA%A8%EB%93%A0%20%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80%20%EC%95%8C%EC%95%84%EC%95%BC%20%ED%95%A0%20SOLID%20%EC%9D%98%20%EC%A7%84%EC%8B%A4%20%ED%98%B9%EC%9D%80%20%EA%B1%B0%EC%A7%93%202a21b16048c280ba8d34c3c476278636/image%203.png)

- 우선 데이터의 구조를 생각해 보면, 상하 스크롤 시에는 데이터의 자료구조가 `List<Content>` 형태이며, 좌우 스와이프 시에는 각 페이지마다 리스트가 필요하므로 `List<List<Content>>` 형태가 필요할 것으로 보입니다. 출력용 데이터 생성 시, 각 화면에 맞는 자료구조를 매핑할 수 있도록 비즈니스 로직을 각각 만들고, 앞서 설명했던 SRP(단일 책임 원칙)를 적용하여 상하 스크롤 화면과 좌우 스와이프 화면의 책임을 분리합니다. 이제 이 흐름도를 이해하기 쉽도록 [그림 6]처럼 간단한 클래스 다이어그램으로 변경해 보겠습니다.

![image.png](%EB%AA%A8%EB%93%A0%20%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80%20%EC%95%8C%EC%95%84%EC%95%BC%20%ED%95%A0%20SOLID%20%EC%9D%98%20%EC%A7%84%EC%8B%A4%20%ED%98%B9%EC%9D%80%20%EA%B1%B0%EC%A7%93%202a21b16048c280ba8d34c3c476278636/image%204.png)

클래스 다이어그램을 보면, 기존의 상하 스크롤 화면(Scroll Fragment)을 그대로 유지하면서, 좌우 스와이프 화면(Swipe Fragment)을 추가할 수 있도록 설계된 것을 알 수 있습니다. 여기에 추가로 고수준 정책 모듈을 보호하기 위해, 이후에 살펴볼 DIP(의존성 역전 원칙)가 적용되어 있습니다.

그러므로 앞으로 새로운 타입의 화면을 추가할 때 수정량을 최소화하기 위해서는 템플릿 메서드 패턴이나 전략 패턴을 사용해 비즈니스 로직을 묶어서 인터페이스로 정의 후, 의존성 주입(DI, Dependency Injection)을 통해 타입과 로직을 매핑하고 동적으로 로드하도록 구현할 수 있습니다. 이후에도 또 다른 요구사항이 생긴다면, 새로운 타입을 정의하고 비즈니스 로직만 추가하면 되어, **기존 코드를 전혀 변경하지 않고도 기능을 확장**할 수 있습니다.

<aside>
💡

즉, 기존의 상하 스크롤 기능에서 변경 하지 않고 Swipe Fragemnt 기능을 구현하게 되었다.

</aside>

```jsx
👨‍🦳 면접관: OCP에 대해 설명 부탁드립니다.

👩🏼‍🦰 지원자: OCP는 개방 폐쇄 원칙으로 기존 수정에는 닫혀 있고, 확장에는 열려 있어야 한다는 원칙입니다.

👨‍🦳 면접관: 답변 감사합니다. 어떻게 하면 기존의 코드를 수정하지 않고 기능을 추가할 수 있을까요?

👩🏼‍🦰 지원자: ...

나라면 ? 
만약 새로운 기능 확장이라면 템플릿 메서드 패턴이나 전략 패턴을 사용해여 공통된 로직을 묶어서 인터페이스로 정의를 합니다.
이후 의존성 주입을 통해 타입과 로직을 매핑하고 동적으로 로드하는 형식으로 구현해 새로운 타입 정의 시 비즈니스 로직만 추가하는 형식으로 OCP 요건을 만족하겠습니다.
```

그렇게 되면 

### **Liskov Substitution Principle(리스코프 치환 원칙)**

> 치환해도 프로그램의 행위가 변하지 않는다.
> 

치환 : 부모 타입을 사용하는 곳 어디에서든 그 자리에 자식 타입(하위)으로 바꿔 넣어도 프로그램이 정상적으로 동작해야 한다.

```jsx
// 부모 클래스 Rectangle
open class Rectangle {
    private var height = 0
    private var width = 0

    fun area() = height * width
    
    open fun setHeight(height: Int) {
        this.height = height
    }

    open fun setWidth(width: Int) {
        this.width = width
    }
}

//하위 클래스 Square
class Square : Rectangle() {
    override fun setHeight(height: Int) {
        this.width = height
        this.height = height
    }

    override fun setWidth(width: Int) {
        this.width = width
        this.height = width
    }
}
```

자식 클래스 Square 가 height와 setwidth가 독립적인 부모 클래스와는 다르게 두 값이 영향을 끼치게 만들어 LSP를 위배하게 된다.

```jsx
fun foo(rect: Rectangle) {
    rect.setHeight(5)
    rect.setWidth(2)
    assert(rect.area() == 10)
}

foo(Rectangle())

foo(Square())//내부 구현이 어떻게 이루어져있는지 모를경우 문제가 발생한다.
```

→ 이 같은 현상은 코드 실행하기 전에 객체의 유형을 학인하고 실행해야 한다.

<aside>
💡

그냥 단순히 상속 관계에서 하위 타입을 설명하는 원칙

</aside>

### ISP **Interface Segregation Principle(인터페이스 분리 원칙)**

> 사용하지 않는 것에 의존하지 않는다.
만약 사용하지 않는 함수가 존재한다면, 인터페이스를 분리해야 한다.
> 

![image.png](%EB%AA%A8%EB%93%A0%20%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80%20%EC%95%8C%EC%95%84%EC%95%BC%20%ED%95%A0%20SOLID%20%EC%9D%98%20%EC%A7%84%EC%8B%A4%20%ED%98%B9%EC%9D%80%20%EA%B1%B0%EC%A7%93%202a21b16048c280ba8d34c3c476278636/image%205.png)

- 모든 유저가 하나의 OPS를 참조하는 데 다음과 같은 단점

모든 사용자가 OPS를 참조하는 경우,
User1은 사용하지 않는 op2(), op3() 까지 참조하고 있따.
만약 , op2() 가 수정되면, 전혀 관계없는 User1이 언어에 따라서 다시 빌드되거나 영향을 받을 수  있다

- 이러한 이유로 인터페이스 분리가 필요.

![image.png](%EB%AA%A8%EB%93%A0%20%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80%20%EC%95%8C%EC%95%84%EC%95%BC%20%ED%95%A0%20SOLID%20%EC%9D%98%20%EC%A7%84%EC%8B%A4%20%ED%98%B9%EC%9D%80%20%EA%B1%B0%EC%A7%93%202a21b16048c280ba8d34c3c476278636/image%206.png)

### DSP **Dependency Inversion Principle(의존성 역전 원칙)**

> 추상화에 의존해야 하며 구체화에 의존하면 안된다.
> 

![image.png](%EB%AA%A8%EB%93%A0%20%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80%20%EC%95%8C%EC%95%84%EC%95%BC%20%ED%95%A0%20SOLID%20%EC%9D%98%20%EC%A7%84%EC%8B%A4%20%ED%98%B9%EC%9D%80%20%EA%B1%B0%EC%A7%93%202a21b16048c280ba8d34c3c476278636/image%207.png)

가잔 안쪽에 변하지 않는 고수준 정책을 배치하고, 바깥쪽으로 갈수록 변동성이 큰 저수준 모듈들을 배치한다.

여기서 많이 보셨을 클린 아키텍처 이미지가 있습니다. 클린 아키텍처는 양파 껍질처럼, **가장 안쪽에 변하지 않는 고수준 정책을 배치하고, 바깥쪽으로 갈수록 변동성이 큰 저수준 모듈들을 배치**합니다. 의존성은 바깥쪽에서 안쪽으로 향하고, 이렇게 하면 바깥쪽의 변화가 안쪽에 영향을 주지 않도록 할 수 있습니다. 따라서 변동성이 큰 저수준 모듈들은 바깥쪽에 자리 잡고, 이들의 변화가 안쪽으로 전파되지 않는 것이 클린 아키텍처의 핵심입니다.

해당 이미지의 우측 하단에 있는 클래스 다이어그램을 보면, 핑크색 화살표의 제어 흐름이 초록색 Controllers 껍질에서 시작해 빨간색 Use Cases 껍질을 거쳐 다시 초록색 Presenter 껍질로 진행되는 것을 알 수 있습니다. 여기서 Use Cases가 Presenter를 참조하기 위해 DIP가 필요합니다. Use Cases 내에 인터페이스를 정의하고, 이를 Presenter에서 구현하여 주입하면 안쪽 껍질에 있는 Use Cases가 Presenter의 기능을 호출할 수 있습니다.

| Layer | 의미 | 포함 내용 |
| --- | --- | --- |
| **Entities** (가장 중심) | 가장 핵심이 되는 Enterprise Business Logic | Domain 모델, 핵심 룰 |
| **Use Cases** | 애플리케이션 Business Logic | 서비스 규칙, 유즈케이스 흐름 |
| **Interface Adapters** | 내부/외부 변환기 역할 | Controller, Presenter, Gateway, DTO |
| **Framework & Drivers** (가장 바깥) | 외부 기술 | DB, UI(Flutter/Android/Web), Network, Framework(Spring/Ktor 등) |