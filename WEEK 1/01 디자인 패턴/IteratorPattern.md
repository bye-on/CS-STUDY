# 이터레이터(Iterator) 패턴

### 의도

이터레이터(iterator)를 사용해여 컬렉션(collection)의 요소들에 접근하는 디자인 패턴. 컬렉션(집합체)의 내부 구조를 노출하지 않고, 원소들을 순차적으로 접근하는 방법을 제공.

NOTE

- 이터레이터 프로토콜: 이터러블한 객체들을 순회할 때 쓰이는 규칙
- 이터러블한 객체: 반복 가능한 객체로 배열을 일반화한 객체

**언제 쓰나**

- 자료구조 내부 표현을 숨기고 싶을 때
- 여러 가지 순회 방법을 같은 컬렉션에 적용하고 싶을 때(예: 전위/중위/후위, 필터링 등)
- 순회 로직을 컬렉션에서 분리해 테스트/변경 용이성을 높이고 싶을 때

**구성**
| 구성요소 | 역할 | Java에서 대응되는 것 |
| ---------------------- | -------------------------------------------------------- | ------------------------------------ |
| **Iterator(반복자)** | 내부 요소를 하나씩 꺼내주는 역할 (`hasNext()`, `next()`) | `java.util.Iterator` |
| **Aggregate(집합체)** | Iterator를 생성해주는 역할 | `java.util.Collection`, `Iterable` |
| **ConcreteAggregate** | 실제 데이터 컬렉션 (ArrayList 등) | `ArrayList`, `HashSet`, `TreeMap` 등 |
| **Client(클라이언트)** | Iterator를 이용해 요소를 사용하는 코드 | `while(it.hasNext())` 부분 |

- Java에선 `java.util.Iterator`, `Iterable`이 표준
- Java의 `Iterator` 인터페이스 자체가
  **디자인 패턴의 “Iterator Pattern”을 기반으로 만든 표준 구현**

---

### 코드 예제 - 이터레이터 패턴을 적용하지 않은 경우

```java
List<String> names = new ArrayList<>();
names.add("A");
names.add("B");
names.add("C");

// 내부 구조(ArrayList)를 알고 있어야만 접근 가능
for (int i = 0; i < names.size(); i++) {
    System.out.println(names.get(i));
}
```

-> 여기서는 **ArrayList의 구조**(`size()`, `get()`)를 알아야만 쓸 수 있음.
즉, 내부 구조가 **노출되어 있음**

---

### 코드 예제 - 이터레이터 패턴을 적용한 경우

```java
List<String> names = List.of("A", "B", "C");
Iterator<String> it = names.iterator();

while (it.hasNext()) {
    System.out.println(it.next());
}
```

-> `names`가 ArrayList인지 LinkedList인지 전혀 몰라도 됨.
그냥 `iterator()`를 통해 **공통된 인터페이스(Iterator)** 로 접근만 하면 됨.

---

**주의/팁**

- **Fail-fast**: `ArrayList`의 이터레이터는 구조적 변경 시 `ConcurrentModificationException`을 던짐. 병렬 순회 필요하면 `CopyOnWriteArrayList`나 `Streams + parallel()` 고려.
- `remove()`는 선택적—직접 구현 시 일관성 유지 필요.
- 필터/맵 변형은 직접 이터레이터로 구현하기보다 **Stream**이 간결한 경우가 많음.
