# 노출 모듈 패턴 (Revealing Module Pattern)

### 의도

**필요한 것만 밖에 보여주고, 나머지는 감춤**
쉽게 말하면 노출 모듈 패턴은 “내부 코드는 숨기고, 외부에는 꼭 필요한 기능(인터페이스)만 보여주는 설계 방법”.

즉, **사용자(다른 개발자)** 는 이 클래스 안에 뭐가 들어있는지 몰라도 필요한 기능만 사용할 수 있게 만드는 것.

### 💡 비유로 이해하기

> “레스토랑 주방”
>
> 손님은 주방 내부(레시피, 조리법, 재료 보관소)를 볼 필요가 없음.
> 대신 **메뉴판(Menu, 인터페이스)** 만 보고 주문.
>
> 주방(내부 구현)은 계속 바뀔 수도 있지만,
> 메뉴판(외부 공개 부분)은 그대로 유지되면 손님은 아무 문제 없음.

---

### 🧩 Java에서의 구현 방법 (2가지)

#### 패키지 레벨에서 숨기기 (일반적인 방식)

- **공개할 인터페이스만 public으로 두고**
- **구체적인 구현 클래스는 같은 패키지 안에서만 보이게(package-private)** 만들어두면 됨.

---

#### 📘 예시

```java
// (1) 외부에 보여줄 인터페이스
package api;

public interface UserService {
    User getById(long id);
    void register(User u);
}
```

```java
// (2) 내부 구현체 (숨김)
package internal;

import api.*;

class UserServiceImpl implements UserService {
    public User getById(long id) { return new User(id, "Arin"); }
    public void register(User u) { System.out.println("등록 완료: " + u.name()); }
}
```

```java
// (3) 공개 팩토리 클래스
package api;

import internal.UserServiceImpl;

public final class UserServices {
    private UserServices() {}
    public static UserService createDefault() {
        return new UserServiceImpl(); // 내부 구현체를 감춘 채로 반환
    }
}
```

```java
// (4) 외부에서 사용하는 쪽
import api.*;

public class App {
    public static void main(String[] args) {
        UserService svc = UserServices.createDefault();
        svc.register(new User(1, "Arin"));
    }
}
```

- 여기서 `App`은 `UserServiceImpl`이란 클래스가 있는지도 모름.
- 그냥 `UserService` 인터페이스만 알고 있음 -> **“노출 모듈 패턴”의 핵심.**

---

#### Java 9 이상에서는 모듈 시스템으로 구현 가능

Java 9부터는 `module-info.java` 파일을 써서
“이 패키지는 공개할게 / 저 패키지는 숨길게”를 직접 설정 가능

```java
module com.example.users {
    exports api;        // api 패키지는 외부 공개
    // exports internal; // internal 패키지는 숨김
}
```

-> 외부 프로젝트에서는 `api` 패키지만 쓸 수 있고,
`internal` 쪽은 접근이 완전히 차단

---

### 왜 쓰는가?

| 이유                       | 설명                                                                | 효과                                                      |
| :------------------------- | :------------------------------------------------------------------ | :-------------------------------------------------------- |
| **보안성 & 안정성**        | 내부 코드가 바뀌어도 외부에서는 그 변화를 몰라도 됨                 | 내부 구조를 자유롭게 수정 가능하고, 외부 코드 영향 최소화 |
| **유지보수 편의성**        | 외부에서 직접 접근하지 못하게 막기 때문에 잘못된 사용을 방지        | 코드 안정성 향상, 실수 줄임                               |
| **캡슐화 (Encapsulation)** | 객체지향의 기본 원칙으로, 필요한 것만 외부에 공개하고 나머지는 숨김 | 코드 구조가 명확해지고, 의존 관계가 단순해짐              |

**주의/팁**

- “노출 모듈”의 핵심은 **표준화된 외피(인터페이스/퍼사드)만 보이게** 하는 것.
- 테스트에선 내부 접근이 필요할 수 있으니 테스트 모듈에서 `opens`/`exports`를 검토.
