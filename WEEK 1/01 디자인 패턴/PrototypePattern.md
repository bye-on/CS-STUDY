## 프로토타입 패턴 (Prototype Pattern)

### 개념
기존 객체를 복제(clone)하여 새로운 객체를 생성하는 패턴, 복잡한 생성 과정을 반복하지 않고 효율적으로 객체를 만든다.
기존의 객체를 새로운 객체로 복사하여 우리의 필요에 따라 수정하는 메커니즘을 제공한다.

### 사용 목적
1. 객체 만드는 비용이 클 때
   - 예: DB에서 데이터를 가져오거나, 복잡한 계산이 필요할 때
2. 비슷한 객체를 여러 개 만들어야 할 때
3. 객체의 타입을 런타임에 결정할 때

### 장점
1. 성능 개선: 복잡한 초기화를 반복하지 않아도 됨
2. 코드 유연성: 새로운 클래스를 만들지 않고도 다양한 객체 생성 가능 → 런타임에 동적으로 객체 구성 변경 가능
3. 캡슐화 유지: 객체 생성의 복잡한 로직을 숨길 수 있음

### 단점
1. 깊은 복사 문제: 객체 안에 객체가 있으면 복사가 복잡해짐
2. 순환 참조 처리의 어려움: 객체들이 서로를 참조하는 경우 `clone()` 구현 시 무한 루프나 중복 복사 문제 발생

### 구현 예시
1. 기본 구현
```
// 1. Cloneable 인터페이스를 구현한 프로토타입
abstract class Shape implements Cloneable {
    private String id;
    protected String type;
    
    abstract void draw();
    
    public String getType() {
        return type;
    }
    
    public String getId() {
        return id;
    }
    
    public void setId(String id) {
        this.id = id;
    }
    
    // 복제 메서드
    @Override
    public Object clone() {
        Object clone = null;
        try {
            clone = super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return clone;
    }
}

// 2. 구체적인 프로토타입 클래스들
class Rectangle extends Shape {
    public Rectangle() {
        type = "Rectangle";
    }
    
    @Override
    void draw() {
        System.out.println("사각형을 그립니다.");
    }
}

class Circle extends Shape {
    public Circle() {
        type = "Circle";
    }
    
    @Override
    void draw() {
        System.out.println("원을 그립니다.");
    }
}

class Triangle extends Shape {
    public Triangle() {
        type = "Triangle";
    }
    
    @Override
    void draw() {
        System.out.println("삼각형을 그립니다.");
    }
}

// 3. 프로토타입을 관리하는 레지스트리
class ShapeCache {
    private static Map<String, Shape> shapeMap = new HashMap<>();
    
    public static Shape getShape(String shapeId) {
        Shape cachedShape = shapeMap.get(shapeId);
        return (Shape) cachedShape.clone();
    }
    
    // 초기 프로토타입들을 캐시에 로드
    public static void loadCache() {
        Circle circle = new Circle();
        circle.setId("1");
        shapeMap.put(circle.getId(), circle);
        
        Rectangle rectangle = new Rectangle();
        rectangle.setId("2");
        shapeMap.put(rectangle.getId(), rectangle);
        
        Triangle triangle = new Triangle();
        triangle.setId("3");
        shapeMap.put(triangle.getId(), triangle);
    }
}

// 4. 사용 예시
public class PrototypePatternDemo {
    public static void main(String[] args) {
        ShapeCache.loadCache();
        
        Shape clonedShape1 = ShapeCache.getShape("1");
        System.out.println("Shape: " + clonedShape1.getType());
        clonedShape1.draw();
        
        Shape clonedShape2 = ShapeCache.getShape("2");
        System.out.println("Shape: " + clonedShape2.getType());
        clonedShape2.draw();
        
        Shape clonedShape3 = ShapeCache.getShape("3");
        System.out.println("Shape: " + clonedShape3.getType());
        clonedShape3.draw();
    }
}
```

2. 깊은 복사
```
class Address implements Cloneable {
    private String city;
    private String street;
    
    public Address(String city, String street) {
        this.city = city;
        this.street = street;
    }
    
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
    
    // getters and setters
    public String getCity() { return city; }
    public void setCity(String city) { this.city = city; }
}

class Person implements Cloneable {
    private String name;
    private int age;
    private Address address;
    
    public Person(String name, int age, Address address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }
    
    // 깊은 복사 구현
    @Override
    protected Object clone() throws CloneNotSupportedException {
        Person cloned = (Person) super.clone();
        // Address 객체도 복제
        cloned.address = (Address) address.clone();
        return cloned;
    }
    
    public void displayInfo() {
        System.out.println("이름: " + name + ", 나이: " + age + 
                         ", 도시: " + address.getCity());
    }
    
    public Address getAddress() { return address; }
}

// 사용 예시
public class DeepCopyExample {
    public static void main(String[] args) throws CloneNotSupportedException {
        Address address = new Address("서울", "강남구");
        Person original = new Person("김철수", 30, address);
        
        Person cloned = (Person) original.clone();
        
        // 복제된 객체의 주소를 변경
        cloned.getAddress().setCity("부산");
        
        System.out.println("원본 객체:");
        original.displayInfo();  // 서울 출력
        
        System.out.println("복제된 객체:");
        cloned.displayInfo();    // 부산 출력
    }
}
```
