# HTTP 1.0

> 한 연결당 하나의 요청을 처리하도록 설계된 방식
> 

TCP 3- Way 핸드셰이크를 계속 열여야 한다.

```jsx
Client                     Server
  | ------- SYN ---------> |
  | <---- SYN + ACK ------ |
  | -------- ACK --------> |
   연결 수립 완료

```

![image.png](HTTP%201%200/image.png)

RTT (Round Trip Time)

패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸리는 시간이며 패킷 왕복 시간

### RTT의 증가를 해결하기 위한 방법

1. 이미지 스플리팅
- Http 1.0 요청은 1번당 1개 연결 → 이미지 서빙이 많은 웹사이트에서 이미지 1장마다 RTT 가 발생을 한다. → 이렇게 되면 서버/네트워크 부하가 증가하고 렌더링이 지연된다.
- 이때 나온 방법이 이미지 스플리팅 방법 (웹 사이트에서 사용하는 여러 이미지들을 하나로 모두 합쳐놓고 CSS 의 background-position을 이용해 부분적으로 필요한 영역만 보여준다.

```java
.icon {
    background-image: url("sprite.png");
    background-repeat: no-repeat;
    width: 50px;
    height: 50px;
}

.icon-home { background-position: -10px -20px; }
.icon-setting { background-position: -60px -20px; }
.icon-cart { background-position: -110px -20px; }
```

1. 이미지 Base64 인코딩
- 이미지 파일을 64 진법으로 이루어진 문자열로 인코딩 하는 방법.
- text 형태로 직접  HTML & CSS 안에 포함시키는 방식이라 이미지를 별도로 서버에서 다운로드 받을 필요가 없다.
- 문서 내부에 포함된 데이터(inlin data) 형식으로 되어 있기 추가로 HTTP GET 요청을 할 필요가 없다.

<aside>
💡

인코딩 

- 정보의 형태나 형식을 표준화, 보안, 처리속도 향상, 저장 공간 절약 등을 위해 다른 형태나 형식으로 변환하는 처리 방식
</aside>