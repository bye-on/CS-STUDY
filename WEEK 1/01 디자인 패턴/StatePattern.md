## 상태 패턴 (State Pattern)

### 개념
객체의 상태 변화에 따라 행동이 달라지는 경우, 상태를 객체로 분리해 조건문 없이 유연하게 상태 전환을 구현하는 패턴

### 사용 목적
1. 객체의 상태에 따라 행동이 달라질 때
2. 상태 전환 로직이 복잡할 때
3. 조건문이 많아질 때
4. 상태별 행동을 명확하게 분리하고 싶을 때

### 사용하지 말아야 할 경우
1. 상태가 2-3개로 매우 적고 단순할 때
2. 상태 전환이 거의 없을 때

### 조건문 비교 vs 상태 패턴 
Before: 조건문 사용
```
class TrafficLight {
    private String state = "RED";
    
    public void changeState() {
        if (state.equals("RED")) {
            System.out.println("빨간불 -> 초록불");
            state = "GREEN";
        } else if (state.equals("GREEN")) {
            System.out.println("초록불 -> 노란불");
            state = "YELLOW";
        } else if (state.equals("YELLOW")) {
            System.out.println("노란불 -> 빨간불");
            state = "RED";
        }
    }
    
    public void display() {
        if (state.equals("RED")) {
            System.out.println("정지!");
        } else if (state.equals("GREEN")) {
            System.out.println("진행!");
        } else if (state.equals("YELLOW")) {
            System.out.println("주의!");
        }
    }
}
```

After: 상태 패턴 사용
```
interface TrafficLightState {
    void change(TrafficLight light);
    void display();
}

class TrafficLight {
    private TrafficLightState state;
    
    public TrafficLight() {
        this.state = new RedLight();
    }
    
    public void setState(TrafficLightState state) {
        this.state = state;
    }
    
    public void changeState() {
        state.change(this);
    }
    
    public void display() {
        state.display();
    }
}

class RedLight implements TrafficLightState {
    @Override
    public void change(TrafficLight light) {
        System.out.println("빨간불 -> 초록불");
        light.setState(new GreenLight());
    }
    
    @Override
    public void display() {
        System.out.println("정지!");
    }
}

class GreenLight implements TrafficLightState {
    @Override
    public void change(TrafficLight light) {
        System.out.println("초록불 -> 노란불");
        light.setState(new YellowLight());
    }
    
    @Override
    public void display() {
        System.out.println("진행!");
    }
}

class YellowLight implements TrafficLightState {
    @Override
    public void change(TrafficLight light) {
        System.out.println("노란불 -> 빨간불");
        light.setState(new RedLight());
    }
    
    @Override
    public void display() {
        System.out.println("주의!");
    }
}
```

### 장점
1. 단일 책임 원칙: 각 상태의 행동이 별도 클래스로 분리됨
2. 개방-폐쇄 원칙: 새로운 상태 추가가 쉬움
3. 가독성 향상: 복잡한 조건문 제거
4. 상태 전환 명확화: 상태 간 전환 로직이 명확함
5. 유지보수 용이: 상태별 코드 수정이 독립적

### 단점
1. 클래스 수 증가: 상태마다 클래스를 만들어야 하므로 클래스 개수가 많아짐
   - 대안: 상태가 3개 이하로 적고 상태 전환 로직이 단순하다면 enum과 switch문을 사용하는 것이 더 간결할 수 있다.

2. 상태 객체 생성 비용: 상태 전환마다 새로운 객체를 생성하면 오버헤드 발생
   - 대안: 상태 객체를 싱글톤으로 만들어 재사용하거나, Flyweight 패턴을 적용하여 미리 생성된 인스턴스를 공유한다.

### 구현 예시
```
interface State {
    void handle();
}

class OnState implements State {
    public void handle() { System.out.println("전원이 켜짐"); }
}

class OffState implements State {
    public void handle() { System.out.println("전원이 꺼짐"); }
}

class Context {
    private State state;
    public void setState(State state) { this.state = state; }
    public void request() { state.handle(); }
}

public class Main {
    public static void main(String[] args) {
        Context context = new Context();
        context.setState(new OnState());
        context.request(); // 전원이 켜짐
        context.setState(new OffState());
        context.request(); // 전원이 꺼짐
    }
}
```
