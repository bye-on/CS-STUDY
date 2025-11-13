# REST(ful) API

---

<aside>
💡

API (Application Programming Interface) 란?
- 어플리케이션과 어플ㄹ리케이션 사이에서 기능 / 데이터를 사용할 수 있게 해주는 “규칙 / 약속 / 인터페이스”
- 두 소프트웨어가 서로 요청과 응답을 주고 받을 때 한 쪽이 어떻게 요청을 보내면 되는지 다른쪽은 어떻게 응답을 하면 되는지 메뉴판처럼 작동하는 것

“네가 이렇게 말하면 내가 이렇게 해줄게!!”

</aside>

# Rest(ful) API

> 현재 가장 많이 사용하는 API 중 하나
리소스 중심의 API 설계 방식
즉,  어떤 행위를 URL 로 표현하는 게 아니라 “대상 (자원/Resource)”을 URL 로 표현하고 동작(행위)는 HTTP 메소드로 표현한다는 철학
> 

<aside>
💡

Rest? Restful? Restful API?
- 위의 철학이 REST
- RESTful - REST 원칙을 잘 지킨 상태를 말하는 형용사
- RESTful API Rest철학을 기반으로 설계된 API

</aside>

[요청/ 응답 방식 & HTTP 요청 방식](%EC%9A%94%EC%B2%AD%20%EC%9D%91%EB%8B%B5%20%EB%B0%A9%EC%8B%9D%20&%20HTTP%20%EC%9A%94%EC%B2%AD%20%EB%B0%A9%EC%8B%9D%202a51b16048c28062ae2bceaabf2b2bc5.md) 

### Rest API 설계 규칙

1. 슬래시 구분자는 (/) 는 계층 관계를 나타내는데 사용된다.
`ex) http://restapi.example.com/houses/apartments`
2. URL 마지막 문자로 슬래시 / 를 포함하지 않는다.
3. 긴 URI 경로 같은 경우 하이픈(-) 을 사용하여 가독성을 높인다.
4. 밑줄(_)은 사용하지 않는다.
5. URL 경로에는 소문자가 적합하다
6. 파일확장자는 URL 에 포함하지 안흔ㄴ다.
- REST API 에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일확장자를 URL 안에 포함시키지 않는다.
- Accep Header 를 사용한다.
`ex) http://restapi.example.com/members/soccer/345/photo.jpg (X)`
`ex) GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg (O)`
7. 리소스 간에는 연관 관계가 있는 경우
- /리소스명/리소스ID/관계가 있는 다른 리소스명

### 예전 방식 VS RESTful 방식

| 옛날 방식(비RESTful) | RESTful 방식 |
| --- | --- |
| /addUser | POST /users |
| /updateUser | PUT /users/10 |
| /deleteUser | DELETE /users/10 |
| /getUserList | GET /users |

### Rest가 왜 필요한데?

- 다양한 클라이언트의 등장으로 애플리케이션 분리 및 통합

→ 안드로이드, 웹 등 다양한 클라이언트의 형태로 나온다.
→ 클라이언트는 다른데 결국 하나의 같은 서버로 요청을 보내야 한다.
→ 이는 통합된 규칙이 필요하고 클라이언트 종류 상관없이 이해 가능한 REST 가 좋다.

```jsx
import org.springframework.web.bind.annotation.*;
import java.util.*;

@RestController
@RequestMapping("/books") // 공통 URI
public class BookController {

    // 임시 데이터 저장용 (실제로는 DB 사용)
    private Map<Integer, String> books = new HashMap<>();

    public BookController() {
        books.put(1, "Spring Boot Guide");
        books.put(2, "Java in Depth");
    }

    // ✅ 1. GET - 조회 (Read)
    @GetMapping("/{id}")
    public String getBook(@PathVariable int id) {
        return books.getOrDefault(id, "Book not found");
    }

    // ✅ 2. POST - 생성 (Create)
    @PostMapping
    public String createBook(@RequestBody Map<String, String> newBook) {
        int newId = books.size() + 1;
        books.put(newId, newBook.get("title"));
        return "Book created with id " + newId;
    }

    // ✅ 3. PUT - 수정 (Update)
    @PutMapping("/{id}")
    public String updateBook(@PathVariable int id, @RequestBody Map<String, String> 
    updatedBook) {
        if (!books.containsKey(id)) {
            return "Book not found";
        }
        books.put(id, updatedBook.get("title"));
        return "Book with id " + id + " updated.";
    }

    // ✅ 4. DELETE - 삭제 (Delete)
    @DeleteMapping("/{id}")
    public String deleteBook(@PathVariable int id) {
        if (!books.containsKey(id)) {
            return "Book not found";
        }
        books.remove(id);
        return "Book with id " + id + " deleted.";
    }
}

```

### Rest 특징

1. **Server-Client ( 서버 - 클라이언트 구조)**
- 자원이 있는 쪽이 Server, 자원을 요청하는 쪽이 Client 가 된다.
- Rest Server : API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
- Client : 사용자 인증이나 Context ( 세션, 로그인 정보) 등을 직접 관리하고 책임진다.
- `이렇게 되면 Server는 자원만 제공 Client 는 표현만 신경 써 의존성이 줄어든다.`
1. **Stateless ( 무상태 )**
- HTTP 프로토콜은 Stateless Protocol이므로 REST 역시 무상태성을 갖는다.
- Client의 context를 Server에 저장하지 않는다.
    - 즉, 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순해진다.
- Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
    - 각 API 서버는 Client의 요청만을 단순 처리한다.
    - 즉, 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.
    - 물론 이전 요청이 DB를 수정하여 DB에 의해 바뀌는 것은 허용한다.
    - Server의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아진다.

<aside>
💡

`Stateless(무상태) 란?`

**서버가 “클라이언트의 대화 맥락(context)”을 기억하지 않는 설계**

각 HTTP 요청은 **서버가 처리하는 데 필요한 모든 정보를 스스로 포함** 해야 하고,

**서버는 그 요청 하나만 보고 판단**해 응답한다

</aside>

<aside>
💡

예시를 들자면, 내가(클라이언트) 서버에 주경이라고 요청을 했다.
서버는 주경이라는 요청을 받고 그에 관련한 DB 작업을 하고 끝낸다.

서버는 요청만 보고 판단하기 때문에 요청이 없는 상태로 너 주경 알아? 하면 못알아 듣는다.
결국 재 요청을 해야한다.

</aside>

```jsx
[Client] → 로그인 요청 (아이디, 비밀번호)
[Server] → JWT 토큰 생성 후 응답
[Client] → 요청 시마다 Authorization 헤더에 JWT 첨부
[Server] → JWT 검증 → 요청 처리

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()  // REST API는 CSRF 불필요
            .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS) // ✅ 핵심! 세션 비활성화
            .and()
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/auth/**").permitAll() // 로그인, 회원가입 등은 허용
                .anyRequest().authenticated()            // 나머지는 인증 필요
            )
            .addFilterBefore(new JwtAuthenticationFilter(), 
            UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}

```

1. **Cacheable ( 캐시 처리 가능 )**
- 웹 표준 HTTP 프로토콜을 그대로 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있다.
    - 즉, HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있다.
    - HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
- 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
- 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.

<aside>
💡

Stateless 이기 때문에 서버는 상태를 들고 있지 않아 부담감이 덜지?
그 상태에서 서버는 자신이 DB 에 작업 한 내용을 캐시에 저장하고 있고 변경 사항이 없다? 그러면 다시 가져와서 재 작업 할 필요없이 그냥 전송만 하면 되는 구조.

</aside>

---

[REST API - 이거 하나로 끝남](https://www.youtube.com/watch?v=fB3MB8TXNXM)

[[간단정리] REST, REST API, RESTful 특징](https://hahahoho5915.tistory.com/54)