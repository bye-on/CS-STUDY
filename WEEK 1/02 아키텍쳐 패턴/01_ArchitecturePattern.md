# 아키텍쳐 패턴(MVC, MVP, MVVM)

MVC, MCP, MVVM 패턴에 대해 알아보자. 

위 셋에 공통적으로 **MV**가 붙는데, 데이터를 담는 Model과 화면에 보여주는 View를 공통으로 가지며 Model과 View를 어떻게 연결할 것이냐에 따라 **C**ontroller, **P**resenter, **V**iew**M**odel 인지를 구분한다.

---

# 아키텍쳐 패턴 vs 디자인 패턴

⇒ 아키텍쳐 패턴이 디자인 패턴보다 더 넓은 수준의 문제를 해결해주며, 디자인 패턴보다 더 추상적이고 상위의 레벨에서 적용된다.

|  | **아키텍처 패턴** | **디자인 패턴** |
| --- | --- | --- |
| **정의** | **소프트웨어 구조를 설계**할 때 자주 나타나는 문제점을 해결하는데 사용하며, 재사용 가능하고 검증된 솔루션 | **소프트웨어 설계**에서 자주 나타나는 문제에 대한, 일반적이고 재사용 가능한 솔루션 |
| **해결해주는 문제** | - 규칙적인 구조를 적용하여 빠르고 효율적인 개발 가능- 안정적인 구조로 유지보수 편리- 원활한 협업 가능 | - 규칙성 있는 구현으로 빠르고 효율적인 개발 가능- 특정 문제를 해결, 예방해주어 유지보수 편리- 원활한 협업 가능 |
| **적용 범위** | 시스템 전체, 전반적인 구조 | 특정 레이어, 맥락, 기능 |
| **종류** | MVC, MVP, MVVM, Service Locator, Publish-subscribe, Client-server 등 | Abstract factory, Singleton, Adapter, Observer, State, Strategy 등 |

## 아키텍쳐 패턴의 종류

- [MVC 패턴](./02_MVC.md)
- [MVP 패턴](./03_MVP.md)
- [MVVM 패턴](./04_MVVM.md)


---

### 참고자료

- https://walnut-dev.tistory.com/7

- https://beomy.tistory.com/43

- https://m.blog.naver.com/jhc9639/220967034588

- https://developer.mozilla.org/ko/docs/Glossary/MVC

- https://velog.io/@inwoong100/React-%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4-MVC-%ED%8C%A8%ED%84%B4%EA%B3%BC-FLUX%ED%8C%A8%ED%84%B4
