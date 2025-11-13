# 요청/ 응답 방식 & HTTP 요청 방식

[[간단정리] HTTP Request/Response 구조](https://hahahoho5915.tistory.com/62)

# Request Message (요청 방식)

> HTTP Request Message 는 공백(blank Line)을 제외하고 3가지 부분으로 나뉜다.
> 

- Start Line
- Headers
- Body

![image.png](%EC%9A%94%EC%B2%AD%20%EC%9D%91%EB%8B%B5%20%EB%B0%A9%EC%8B%9D%20&%20HTTP%20%EC%9A%94%EC%B2%AD%20%EB%B0%A9%EC%8B%9D/image.png)

> Start Line
> 
- HTTP Request Message 의 시작 라인 (3가지로 구성)
- Http Method
- Request Target
- Http version

```java
GET /test.html HTTP/1.1
[HTTP Method] [Request target] [HTTP version]
```

- `HTTP method`는 요청의 의도를 담고 있는 GET, POST, PUT, DELETE 등이 있다. GET은 존재하는 자원에 대한 요청, POST는 새로운 자원을 생성, PUT은 존재하는 자원에 대한 변경, DELETE는 존재하는 자원에 대한 삭제와 같은 기능을 가지고 있다.

[HTTP 메서드](%EC%9A%94%EC%B2%AD%20%EC%9D%91%EB%8B%B5%20%EB%B0%A9%EC%8B%9D%20&%20HTTP%20%EC%9A%94%EC%B2%AD%20%EB%B0%A9%EC%8B%9D/HTTP%20%EB%A9%94%EC%84%9C%EB%93%9C%202a51b16048c280ad9b2df72effbe7ebc.md)

- `Request target`은 HTTP Request가 전송되는 목표 주소
- `HTTP version`은 version에 따라 Request 메시지 구조나 데이터가 다를 수 있어서 version을 명시한다.

> Headers
> 
- 해당 request 에 대한 추가 정보 (additional information)를 담고 있는 부분
- Ex) request 메시지 body 의 총 길이 (Context-Length) 등 Key : Value 형태로 구성되어 있다 명시
- General Headers, Request Headers, Enitity Headers로 크게 3가지로 구성

```java
Host: google.com
Accept: text/html
Accept-Encoding: gzip, deflate
Connection: keep-alive
...
```

| `Host`  | 요청하려는 서버 호스트 이름과 포트번호 |
| --- | --- |
| `User-agent` | 클라이언트 프로그램 정보
해당 정보를 통해 서버는 클라이언트 프로그램(브라우저)에 맞는 최적의 데이터를 보내줄 수 있다) |
| `Refer` | 바로 직전에 머물렀던 웹 링크 주소 |
| `Accept` | 클라이언트가 처리 가능한 미디어 타입 종류 나열 |
| `If-Modified-Since` | 여기에 쓰여진 시간 이후로 변경된 리소스 취득
페이지가 수정되었으면 최신 페이지로 교체한다. |
| `Authorization` | 인증 토큰을 서버로 보낼 때 쓰이는 Header |
| `Origin` |  서버로 Post 요청을 보낼 때 요청이 어느 주소에 시작되어있는지 나타내는 값.
이 값으로 요청을 보낸 주소와 받는 주소가 다르면 CROS(Cross-Origin: Resource Sharing) 에러가 발생한다. |
| `Cookie` | 쿠키 값이 key-value로 표현된다. |

> Body
> 
- HTTP Request 가 전송하는 데이터를 담고 있는 부분
- 전송하는 데이터가 없다면 body 부분은 비어있다.
- 보통 Post 요청일 경우 , HTML 폼 데이터가 포함되어 있다.

```java
POST /test HTTP/1.1

Accept: application/json
Accept-Encoding: gzip, deflate
Connection: keep-alive
Content-Length: 83
Content-Type: application/json
Host: google.com
User-Agent: HTTPie/0.9.3

{
    "test_id": "tmp_1234567",
    "order_id": "8237352"
}
```

# Response Message (응답 방식)

> 공백 (Blank Line) 을 제외하고 3가지 부분으로 나누어진다.
> 
- Statuc Line
- Headers
- Body

```java
HTTP/1.1 200 OK
[HTTP version] [Status Code] [Status Text]
```