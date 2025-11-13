# SoAP Vs RestAPI

---

### SOAP

> Simple Object Access Protocol
> 

> 모든 작업들이 지정된 한 주소로 이요한다.
> 

- REST API

![image.png](SoAP%20Vs%20RestAPI/image.png)

- GET 으로 명시하고 URI에서 관련 정보를 담겨져 잇는다.

- SOAP

![image.png](SoAP%20Vs%20RestAPI/image%201.png)

- XML 기반
- BODY 에 정보를 담아 POST 를 주로 사용한다.

### 보안 표준

| 항목 | SOAP | REST |
| --- | --- | --- |
| **보안 표준** | WS-Security, XML Encryption 등 고도화된 보안 가능 | HTTPS로 기본 암호화, 별도 표준은 없음 |
| **트랜잭션** | ACID 트랜잭션 지원 | 일반적으로 지원하지 않음 |
| **에러 처리** | SOAP Fault 메시지 구조로 표준화됨 | HTTP 상태 코드(200, 404, 500 등) 활용 |

### 사용 사례

| 항목 | SOAP | REST |
| --- | --- | --- |
| **주로 사용되는 분야** | 금융, 결제, 기업용 시스템 등 **보안과 표준**이 중요한 곳 | 모바일 앱, 웹 서비스, SNS API 등 **속도와 유연성**이 필요한 곳 |
| **예시** | 은행 시스템, 기업 간 B2B 서비스 | Twitter API, GitHub API, Google Maps API |