# 빌더(Builder) 패턴

### 의도

복잡한 객체 생성을 단계적으로 수행하고, **가독성 높은 불변 객체**를 만들기 쉽게 함. (텔레스코픽 생성자 문제 해결)

위의 문장을 조금 더 쉽게 풀면

**빌더 패턴(Builder Pattern)** 은 “복잡한 객체를 한 번에 만드는 대신, 필요한 값을 하나씩 차근차근 넣어서 만드는 방법.”

그리고 이렇게 하면
“보기 쉽고, 수정하기 쉬우며, 한 번 만들어진 뒤에는 변하지 않는(불변) 객체를 만들 수 있음.”

**언제 쓰나**

- 선택적 파라미터가 많을 때
- 생성 중 **검증/파생값 계산**이 필요할 때
- 불변(immutable) 객체를 만들고 싶을 때

---

### 상황 예시

만약 어떤 사람 정보를 담는 `Person` 클래스를 만든다고 해볼게요.
필요한 정보가 많으면 생성자가 너무 복잡해짐

```java
Person p = new Person("Same", 25, "US", "Developer", "same@abc.com");
```

이건 보기에도 헷갈리고, 파라미터 순서도 틀리기 쉬움.
이런 걸 **텔레스코픽 생성자 문제**라고 부름.
(텔레스코픽 = 망원경처럼 길게 늘어져서 보기 불편하다는 뜻)

### 빌더 패턴으로 바꾸면

```java
Person p = new Person.Builder()
    .name("Same")
    .age(25)
    .country("US")
    .job("Developer")
    .email("same@abc.com")
    .build();
```

### 핵심 정리

| 개념                            | 설명                                                                  |
| ------------------------------- | --------------------------------------------------------------------- |
| **단계적으로 만든다**           | 한 번에 다 넣는 게 아니라 `builder()`로 필요한 속성을 하나씩 채워나감 |
| **가독성 높다**                 | 코드를 읽을 때 “무슨 값이 어디 들어가는지” 바로 알 수 있음            |
| **불변 객체**                   | 한 번 만들어지면 내용이 바뀌지 않아 안전함 (`final` 필드)             |
| **텔레스코픽 생성자 문제 해결** | 생성자에 파라미터가 너무 많아서 생기는 혼란을 없앰                    |

---

## 코드 예제 1

HttpRequest라는 클래스(예: 서버에 보낼 요청 객체)를 만들 때 필수 값(method, url) 과 선택 값(timeout, redirect 설정) 을 한 번에 깔끔하게 설정하기 위해 쓰는 코드

```java
public final class HttpRequest {
    // ✅ 1. 불변(immutable) 객체의 실제 데이터
    private final String method;   // 필수
    private final String url;      // 필수
    private final int timeoutMs;   // 선택
    private final boolean followRedirects; // 선택

    // ✅ 2. 생성자는 private → 외부에서 직접 만들 수 없음
    private HttpRequest(Builder b) {
        this.method = b.method;
        this.url = b.url;
        this.timeoutMs = b.timeoutMs;
        this.followRedirects = b.followRedirects;
    }

    // ✅ 3. 내부에 Builder 클래스를 둠
    public static class Builder {
        private final String method;
        private final String url;
        private int timeoutMs = 5000; // 기본값
        private boolean followRedirects = true; // 기본값

        // (1) 필수값은 생성자에서 받음
        public Builder(String method, String url) {
            if (method == null || url == null)
                throw new IllegalArgumentException("method/url required");
            this.method = method;
            this.url = url;
        }

        // (2) 선택값은 메서드 체이닝으로 추가
        public Builder timeoutMs(int v) {
            this.timeoutMs = v;
            return this; // this를 반환해서 .으로 연결 가능
        }

        public Builder followRedirects(boolean v) {
            this.followRedirects = v;
            return this;
        }

        // (3) 마지막에 build()로 완성!
        public HttpRequest build() {
            return new HttpRequest(this);
        }
    }

    // ✅ 4. 완성된 객체는 getter만 있고, 값 변경 불가
    public String method() { return method; }
    public String url() { return url; }
    public int timeoutMs() { return timeoutMs; }
    public boolean followRedirects() { return followRedirects; }
}


// 사용 예시
// 결과적으로 req는 완성된 불변 객체
// req.url()처럼 값을 읽을 수만 있고, 바꿀 수 X
public class Demo {
    public static void main(String[] args) {
        HttpRequest req = new HttpRequest.Builder("GET", "https://example.com")
                .timeoutMs(10000)        // 선택적으로 추가 설정
                .followRedirects(false)  // 선택적으로 설정
                .build();                // 최종 객체 완성!

        System.out.println(req.url());
    }
}

```

---

## 코드 예제 2

**스텝 빌더(순서 강제, 선택/필수 분리)**
이건 **“값을 반드시 순서대로 넣게 강제하는 빌더”**. 보통 빌더는 순서 상관없이 설정할 수 있는데, 어떤 필드는 반드시 입력되어야 할 때. 그래서 이런 식으로 “단계별 인터페이스”를 나눔

```java
interface MethodStep { UrlStep method(String m); }  // 1단계
interface UrlStep { OptionalStep url(String u); }   // 2단계
interface OptionalStep {                            // 3단계 (선택)
    OptionalStep timeoutMs(int v);
    OptionalStep followRedirects(boolean v);
    HttpRequest build();
}

// method()를 먼저 호출하지 않으면 url()도, build()도 호출할 수 없게 컴파일 단계에서 막아줌.
// “필수 값은 반드시 순서대로 입력해야 한다” 라는 타입 안전성(type safety) 을 보장하는 버전.
class HttpRequestBuilders {
    public static MethodStep builder() {
        return m -> u -> new OptionalStep() {
            String method = m, url = u; int timeout = 5000; boolean follow = true;
            @Override public OptionalStep timeoutMs(int v){ timeout = v; return this; }
            @Override public OptionalStep followRedirects(boolean v){ follow = v; return this; }
            @Override public HttpRequest build(){
                return new HttpRequest.Builder(method, url).timeoutMs(timeout).followRedirects(follow).build();
            }
        };
    }
}

```

---

### 왜 빌더 패턴을 쓰는가?

| 번호 | 문제 상황                            | 예시 코드                                                                          | 빌더 패턴 사용 시 장점                                                                                                |
| :--: | :----------------------------------- | :--------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------- |
|  1️⃣  | **생성자가 너무 길어서 보기 어렵다** | `new HttpRequest("GET", "https://example.com", 10000, false);`                     | 어떤 값이 무슨 역할인지 직관적으로 알기 어렵다 → 빌더를 쓰면 각 속성 이름을 명시적으로 설정할 수 있어 가독성이 높아짐 |
|  2️⃣  | **선택적인 값이 많을 때 불편하다**   | 예: timeout, redirect 같은 설정이 있을 때마다 오버로딩된 생성자를 계속 만들어야 함 | 빌더는 “필요한 값만 설정”하고 나머지는 기본값 사용 가능 (`timeoutMs()`만 호출해도 됨)                                 |
|  3️⃣  | **코드 가독성이 떨어진다**           | 여러 개의 매개변수가 이어지면 순서를 혼동하기 쉽고 의미를 파악하기 어려움          | 빌더는 체이닝(`.timeoutMs().followRedirects()`)으로 의미를 바로 파악할 수 있어 읽기 쉽고 유지보수도 쉬움              |

---

**주의/팁**

- **검증은 `build()`에서** 한 번에(불완전 상태 노출 방지)
- 상속 구조의 빌더는 복잡해지기 쉬움 -> 합성(필드 포함) + 단일 빌더 추천
- Lombok `@Builder`로 보일러플레이트 감소 가능(단, 런타임 의존/리플렉션 정책 고려)
