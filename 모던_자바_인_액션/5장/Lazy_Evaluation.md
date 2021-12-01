# Lazy Evaluation

## Lazy Evaluation이란?

* 실제 필요한 경우에 연산을 시작

> 반대는 Eager Evaluation



### 예시

```java
public static void main(String[] args) {
    long startTime = System.currentTimeMillis();

    methodResult(true, getExpensiveValue());
    methodResult(false, getExpensiveValue());
    methodResult(false, getExpensiveValue());

    System.out.println("passed Time: " + (System.currentTimeMillis() - startTime) / 1000 + "sec\n");

    startTime = System.currentTimeMillis();
		supplier(true, () -> getExpensiveValue());
    supplier(false, () -> getExpensiveValue());
    supplier(false, () -> getExpensiveValue());
    System.out.println("passed Time: " + (System.currentTimeMillis() - startTime) / 1000 + "sec");
}

private static void methodResult(boolean valid, String value) {
    if (valid)
        System.out.println("true: " + value);
    else
        System.out.println("false");
}

private static String getExpensiveValue() {
    try {
        TimeUnit.SECONDS.sleep(1);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    return "Hello World";
}

private static void supplier(boolean valid, Supplier<String> valueSupplier) {
    if (valid)
        System.out.println("true: " + valueSupplier.get());
    else
        System.out.println("false");
}
```

```
true: Hello World
false
false
passed Time: 3sec

true: Hello World
false
false
passed Time: 1sec
```

>  [출처](https://dororongju.tistory.com/137)

위의 코드에서 **메서드**가 파라미터인 경우, valid에 상관없이 무조건 실행이 된다. 총 세 번이 실행되어 3초가 걸린다.

하지만 **람다식**이 파라미터인 경우 valid가 false인 경우, 실행되지 않는다. 

출력은 같지만 걸린 시간이 다르다. **Lazy Evaluation**은 이렇게 불필요한 연산을 하지 않음으로써 성능을 향상 시킨다.





```java
public static void main(String[] args) {
    service();
    System.out.println("\n======\n");
    streamService();
}
public static void service() {
    List<Integer> integers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15);
    List<Integer> filteredInteger = filter(integers);
    System.out.println("result = " + Arrays.toString(filteredInteger.toArray()));
}

private static List<Integer> filter(List<Integer> integers) {

    List<Integer> result = new ArrayList<>();

    int count = 0;
    for (Integer i : integers) {
        System.out.println("filtering_1: i = " + i);
        if (i % 3 == 0 && count++ < 3) {
            result.add(i * 10);
        }
    }

    return result;
}

public static void streamService() {
    List<Integer> integers_2 = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15);
    List<Integer> filteredInteger_2 = streamFilter(integers_2);
    System.out.println("result = " + Arrays.toString(filteredInteger_2.toArray()));
}

static int count = 0;

private static List<Integer> streamFilter(List<Integer> integers) {
    return integers.stream()
                    .filter(i -> {
                        System.out.println("filtering_2: i = " + i);
                        return i % 3 == 0;
                    })
                    .map(new Function<Integer, Integer>() {
                        @Override
                        public Integer apply(Integer i) {
                            System.out.println(++count);
                            return i * 10;
                        }
                    })
                    .limit(3)
                    .collect(Collectors.toList());
}
```

```
filtering_1: i = 1
filtering_1: i = 2
filtering_1: i = 3
filtering_1: i = 4
filtering_1: i = 5
filtering_1: i = 6
filtering_1: i = 7
filtering_1: i = 8
filtering_1: i = 9
filtering_1: i = 10
filtering_1: i = 11
filtering_1: i = 12
filtering_1: i = 13
filtering_1: i = 14
filtering_1: i = 15
result = [30, 60, 90]

======

filtering_2: i = 1
filtering_2: i = 2
filtering_2: i = 3
1
filtering_2: i = 4
filtering_2: i = 5
filtering_2: i = 6
2
filtering_2: i = 7
filtering_2: i = 8
filtering_2: i = 9
3
result = [30, 60, 90]
```

위의 예제는 1~15의 정수가 있는데, 3의 배수만 필터링 하고, 10을 곱한 뒤 앞의 3가지만 가져오는 코드다.

일반적인 메서드를 사용하면 1부터 15까지 총 15번 반복문을 탐색후 결과를 처리한다.

반면, 스트림은 `limit` 메서드를 통해 목표한 3개가 필터링 되면 탐색을 종료한다. 또한 `filter`메서드를 통과하지 못한 값도 `map`메서드에서 실행되지 않는다. 이것 또한 Lazy Evaluation을 통해 성능을 향상시키는 것이다.



>  [출처](https://sabarada.tistory.com/154)