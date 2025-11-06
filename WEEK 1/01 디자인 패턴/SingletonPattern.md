## 싱글톤 패턴 (Singleton Pattern)

### 개념
하나의 클래스에 대해 오직 하나의 인스턴스만 존재하도록 보장하고, 전역적으로 접근할 수 있도록 하는 패턴

### 사용 목적
1. 리소스를 공유해야 하는 경우
   - 공유 자원의 인스턴스를 매번 새로 만들면 리소스 낭비가 크다.
   - 예: 데이터베이스 연결 객체, 로그 기록 객체

2. 전역적으로 동일한 상태를 유지해야 하는 객체가 필요한 경우

### 주요 특징
1. 유일성: 클래스의 인스턴스가 하나만 존재
2. 전역 접근: 어디서든 해당 인스턴스에 접근 가능

### 장점
1. 메모리 절약: 인스턴스를 하나만 생성
2. 전역 접근: 어디서든 동일한 인스턴스 사용
3. 데이터 공유 용이

### 단점
1. 테스트 어려움: 전역 상태로 인한 단위 테스트 복잡도 증가
   - 대안: 의존성 주입을 통해 생성자나 메서드 파라미터로 필요한 객체를 전달받는다.

2. 결합도 증가: 싱글톤을 사용하는 모든 클래스가 해당 클래스에 강하게 결합
   - 대안: 인터페이스를 정의하고 의존성 주입을 활용한다. 클래스가 구체적인 싱글톤 클래스가 아닌 인터페이스에 의존하도록 하면, 구현체를 쉽게 교체할 수 있고 결합도가 낮아진다.

### 구현 방법

#### 1. Eager Initialization (이른 초기화)
```
public class Singleton {
    // 클래스 로딩 시점에 인스턴스 생성
    private static final Singleton instance = new Singleton();
    
    private Singleton() {
        // private 생성자로 외부 생성 방지
    }
    
    public static Singleton getInstance() {
        return instance;
    }
}
```
장점: 구현이 간단하고 thread-safe  
(Thread Safe : 여러 스레드가 동시에 접근하는 경우 해당 애플리케이션에 어떠한 문제도 발생하지 않는 것)
  
단점: 사용하지 않아도 인스턴스가 생성됨

#### 2. Lazy Initialization (늦은 초기화)
```
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
장점: 필요할 때만 인스턴스 생성  
단점: 멀티스레드 환경에서 안전하지 않음

#### 3. Thread-Safe Singleton (동기화)
```
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
장점: 멀티스레드 환경에서 안전  
-> sychronized 키워드를 통해 getInstance() 메서드에 하나의 스레드만 접근 가능하도록 한다.  
  
단점: synchronized로 인한 성능 저하  
-> 여러 스레드가 getInstance() 메서드를 동시에 접근하려 할 때 동기화 작업(Lock) 때문에 성능 저하가 발생할 수 있다.

#### 4. Double-Checked Locking
```
public class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
장점: 성능과 thread-safety 모두 확보  
주의: volatile 키워드 필수

#### 5. Bill Pugh Singleton (권장)
```
public class Singleton {
    private Singleton() {}
    
    private static class SingletonHelper {
        private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```
장점: Lazy initialization + thread-safe + 성능 우수    
-> 내부 클래스는 getInstance() 호출 시점에 로딩됨

#### 6. Enum Singleton (가장 안전)
```
public enum Singleton {
    INSTANCE;
    
    public void doSomething() {
        // 비즈니스 로직
    }
}

public static void main(String[] args) {
    Singleton singleton = Singleton.INSTANCE;
    System.out.println(singleton.doSomething());
}
```
장점:  
- 직렬화/역직렬화에도 안전  
- 리플렉션 공격 방어  
- 구현이 간단
