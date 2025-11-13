# HTTP / 1.1

> 1.0 에서 발전한 것.
매번 TCP 연결을 하는 것이 아니라 한 번 TCP 초기화를 한 이후에 Keep-alive 옵션으로 여러 개의 파일을 송수신할 수 있게 바뀌었다.
< 1.0 버전에서도 keep-alive가 있었지만 표준화가 되어있지 않았고 1.1부터 표준화가 기본 옵션으로 설정되어 있었다.
> 

<aside>
💡

근데 그럼 1.0 에서부터 Keep Alive를 쓰면 되는거 아닌가?

→ 언제 끊는지/ time out/ connection 관리 정책이 통일되지 않았음.

브라우저/ 서버 / proxy 마다 keep-alive구현 방식이 다 달랐음 (표준화가 되어 있지 않았기 때문에 ) 
→ 그렇기 때문에 1.0 버전에서는 안정적 동작 보장이 안되고, 상호 운용성이 너무 안좋았다.

</aside>

![image.png](HTTP%201%201/image.png)

TCP Handshake : 클라이언트와 서버가 통신 가능 상태라는 걸 확인하고 연결하는 과정

TearDown : 종료 과정

### HOL Blocking (Head Of Line Blocking)

> 네트워크에서 같은 큐에 있는 패킷이 그 첫번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상을 말한다.
> 

![image.png](HTTP%201%201/image%201.png)

image.jpg 와 styles, css, data, xml 을 다운로드 받을 때 보통은 순차적으로 잘 받아지지만 image.jpg 가 느리게 받아진다면 그 뒤에 있는 것들이 대기하게 되며 다운로드가 지연되는 상태가 된다.

### 단점

Http/1.1 헤더에는 쿠키 등 많은 메타데이터가 들어 있고 압축이 되지 않아 무거웠다.