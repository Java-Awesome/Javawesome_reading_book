# chapter02 코드 스타일 레벨 업

> 훌륭한 코드는 짧고 단순하고 대칭을 이룬다. 문제는 어떻게 그렇게 하느냐다. - 숀 파렌트

## 2.1 매직 넘버를 상수로 대체

프로그래머는 코드에서 옵션 집합을 표현할 때 종종 숫자 집합을 사용한다. 표면상 의미가 없는 숫자이지만 프로그램의 동작을 제어한는 이 숫자를 **매직 넘버**라고 한다.

```java
double area = r * r * 3.14 // 매직 넘버 3.14
```

이러한 부분을 상수로 대체하면 각 숫자마다 유의미하고 이해하기 쉬운 `상수`로 표현할 수 있다.

```java
static final double PI = 3.14;
```

이러한 변수는 `static` 이고 `final`이다. 즉 `상수(constant)`이다. 변수는 딱 학 번만 존재하게 하고 변경될 수 없게 한다. 자바 코드 규칙에 따라 상수명은 모두 대문자로 표기한다.

```java
double area = r * r * PI; // 상수로 대체
```

## 2.2 정수 상수 대신 열거형

매직 넘버 보단 상수가 훨씬 났다. 하지만 옵션을 모두 열거할 수 이다면 자바 타입 시스템이 제공하는 방법이 더 낫다.

```java
enum Size {

    M(95),
    L(100),
    XL(105);

    final int round;

    Size(int round) {
        this.round = round;
    }
}
```

자바 타입 시스템은 가끔 들어오는 유효하지 않은 입력값을 막는 데 큰 역할을 한다.가능한 옵션을 모두 열거할 수 있다면 항상 정수 대신 enum 타입을 사용하는 것이 좋다.

## 2.3 For 루프 대신 For-Each

인덱스 변수가 제공하는 정보를 자세히 알아야 하는 경우는 드물다. 이럴 때는 루프를 다르게 즉 세부 순회 내용은 보호할 수 없지만 적어도 숨기는 식으로 작성해야 한다.

```java
List<String> list = new ArrayList<>();
list.add("java");
list.add("kotlin");
list.add("groovy");
list.add("javascript");


for (String s : list) {
    System.out.println(s);
}
```

매 반복마다 자바는 자료 구조에서 새로운 객체를 가져와 check에 할당한다. 반복 인덱스를 더 이상 다루지 않아도 된다. 배열과 Set 처럼 인덱싱 되지 않은 컬렉션에도 동작한다.

## 2.4 순회하며 컬렉션 수정하지 않기

```java
List<String> list = new ArrayList<>();
list.add("java");
list.add("kotlin");
list.add("groovy");
list.add("javascript");


for (String s : list) {
    if (isJVM(s)) {
        list.remove(s);
    }
}
```
코드에서 배열이나 리스트를 비롯해 다양한 자료 구조를 순회한다. 대부분의 자료 구조는 읽기만 한다. 하지만 자료 구조를 바꾸려면 조심해야 한다.

문제는 `remove`부분이다. 이렇게 실행하면 List 인터페이스의 표준 구현이나 Collection 인터페이스의 구현은 `ConcurrentModificationException`을 던진다. List를 순회하면서 List를 수정할 수 없다.

Collection을 순회하는 동안 그 컬렉션을 수정한다는 의미이다. Java의 컴파일 타임 검사로는 이 오류를 잡아내지 못한다.

```java
List<String> list = new ArrayList<>();
list.add("java");
list.add("kotlin");
list.add("groovy");
list.add("javascript");

Iterator<String> iterator = list.iterator();
while(iterator.hasNext()) {
    iterator.remove();
}
```

## 2.5 순회하며 계산 집약적 연산하지 않기

```java
class Inventory {

    private List<Supply> supplies = new ArrayList<>();

    List<Supply> find(String regex) {
        List<Supply> result = new LinkedList<>();
        for (Supply supply : supplies) {
            if (Pattern.matches(regex, supply.toString())) {
                result.add(supply);
            }
        }
    }

    return result;
}
```

위 방법은 유용하지만 성능을 저하시키는 요인이다. 코드를 실행하면서 Java는 String 표현식인 regex를 가져와 regex로 부터 특수한 목적의 오토마톤을 만든다. 정규식 오토마톤 컨파일은 클래스 컴파일 처럼 시간과 처리 전력을 소모한다. 위 예제에서는 이러한 소모를 매 반복 마다 처리하고 있다.

```java
class Inventory {

    private List<Supply> supplies = new ArrayList<>();

    List<Supply> find(String regex) {
        List<Supply> result = new LinkedList<>();
        Pattern pattern = Pattern.compile(regex);
        for (Supply supply : supplies) {
            if (Pattern.matcher(supply.toString()).matches()) {
                result.add(supply);
            }
        }
    }

    return result;
}
```

계산이 많이 필요한 연산을 순회 전에 처리한다. 성능은 크게 향상될 수 있다.

## 2.6 새 줄로 그루핑

```java
enum Size {

    M(95),
    L(100),
    XL(105);
    final int round;
    Size(int round) {
        this.round = round;
    }
}
```


연관된 코드와 개념은 함께 그루핑하고 서로 다른 그룹은 빈 줄로 각각 분리해야 한다.

```java
enum Size {

    M(95),
    L(100),
    XL(105);

    final int round;

    Size(int round) {
        this.round = round;
    }
}
```

수직 공간이라는 개념은 훨씬 더 확장된 개념이다. 로버트 C, 마틴은 자신의 저서 <클린 코드>에서 수직 서식화를 신문에 비유했다. 훌륭한 기사는 제목(클래스 명)으로 시작해 섹션 머릿말(공개 멤버, 생성자, 메소드)에 이어서 세부 내용(비공개 메소드)이 나온다. 코드를 이렇게 조직하면 코드를 읽어 내려가기만 해도 이미 클래스를 훨씬 더 쉽게 이해할 수 있다. 클래스에서 기능을 찾기도 훨씬 쉬워진다.

## 2.7 이어붙이기 대신 서식화

긴 문자열을 생성할 때 서식 문자열을 사용하면 더 읽기 쉽게 만들 수 있다. 

핵심은 어떻게 출력할지와 무엇을 출력할지 분리하는 것이다.

```java
System.out.printf("%d + %d = %d", 3, 5, 3 + 5);
```

## 2.8 직접 만들지 말고 자바 API 사용하기

```java
if (supply == null) {
    throw new NullPointerException("supply must be null");
}
```

```java
Objects.requiredNonNull(supply, "supply must not be null");
```

같은 결과를 생성하지만 직접 작성한 코드 대신 자바 API 기능을 사용했다. 짧아진 만큼 이해하기 더 쉽다.

