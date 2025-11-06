## 팩토리 패턴 (Factory Pattern)

### 개념
객체 생성 코드를 별도의 팩토리 클래스로 분리해 구체적인 생성 과정을 몰라도 객체를 얻을 수 있도록 하는 패턴

### 문제점
개방-폐쇄의 원칙 : 확장에 있어서는 열려 있어야 하며, 수정에 있어서는 닫혀 있어야 한다.  
즉, 코드를 수정하지 않아도 모듈의 기능을 확장하거나 변경 할 수 있어야 한다.  
  
결합도가 높다면 코드를 수정했을 때 다른 클래스에 주는 영향이 크다.  
예를 들어 Product의 생성자를 바꾼다면 각각의 User 클래스에 있는 모든 Product 객체의 생성자를 변경해 주어야 한다.  

팩토리 패턴을 적용하면 User 클래스 내부에서 Product 객체를 직접 생성하지 않는다.  
Product의 생성자가 변경된다면? Factory()의 getInstance() 메소드 내부에 있는 Product 생성자만 변경 시켜주면 된다.

### 사용 목적
1. 객체 생성 로직 캡슐화
2. 객체 생성 과정이 복잡할 때
3. 생성할 객체의 타입이 런타임에 결정될 때
4. 코드의 결합도를 낮추고 유연성을 높일 때

### 사용하지 말아야 할 경우
1. 객체 생성이 단순하고 변경 가능성이 낮을 때
2. 생성할 객체의 종류가 하나뿐일 때

### 장점
1. 결합도 감소: 클라이언트 코드가 구체적인 클래스에 의존하지 않음
2. 단일 책임 원칙: 객체 생성 코드를 한 곳에 모을 수 있음
3. 개방-폐쇄 원칙: 기존 코드 수정 없이 새로운 제품 추가 가능
4. 유연성: 생성할 객체의 타입을 쉽게 변경 가능

### 단점
1. 코드 복잡도 증가: 간단한 객체 생성에도 패턴 적용을 위해 많은 클래스와 인터페이스가 필요함
   - 대안: 객체 생성 로직이 복잡하거나 자주 변경되는 경우에만 팩토리 패턴 적용 권장

2. 의존성 관리 복잡성: 팩토리 자체에 대한 의존성 관리가 필요함
   - 대안: DI(의존성 주입) 프레임워크를 사용하면 팩토리 패턴의 장점을 유지하면서도 의존성 관리를 자동화할 수 있다.  
           Spring의 `@Bean`이나 `@Component`를 활용하면 명시적인 팩토리 클래스 없이도 유연한 객체 생성이 가능하다.

### 구현 예시
```
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() { System.out.println("Circle"); }
}

class Triangle implements Shape {
    public void draw() { System.out.println("Triangle"); }
}

class ShapeFactory {
    public Shape getShape(String type) {
        if (type.equals("circle")) return new Circle();
        else if (type.equals("triangle")) return new Triangle();
        return null;
    }
}

public class Main {
    public static void main(String[] args) {
        Shape shapeC = new ShapeFactory().getShape("circle");
        shapeC.draw();

        Shape shapeT = new ShapeFactory().getShape("triangle");
        shapeT.draw();
    }
}
```
