# Stream

## Stream 생성

### Collection으로부터 Stream 생성

Collection 인터페이스에는 stream()이 정의되어 있기 때문에, Collection 인터페이스를 구현한 객체들(List, Set 등)은 모두 이 메소드를 이용해 Stream을 생성할 수 있다. stream()을 사용하면 해당 Collection의 객체를 소스로 하는 Stream을 반환한다.

```java
List<String> list = Arrays.asList("가", "나", "다"); 
Stream<String> listStream = list.stream();
```

### 배열을 Stream으로 생성

배열의 원소들을 소스로하는 Stream을 생성하기 위해서는 Stream의 of 메소드 또는 Arrays의 stream 메소드를 사용하면 된다.

```java
Stream<String> stream1 = Stream.of("가", "나", "다"); 
Stream<String> stream2 = Stream.of(new String[] {"가", "나", ); 
Stream<String> stream3 = Arrays.stream(new String[] {"가",  "다"}); 
Stream<String> stream4 = Arrays.stream(new String[] {"가", "나", "다"}, 0, 3); // 마지막 범위 지정
```

### 원시 Stream 생성

int와 long 그리고 double과 같은 원시 자료형들을 사용하기 위한 특수한 종류의 Stream(IntStream, LongStream, DoubleStream) 들도 사용할 수 있다. Intstream같은 경우 range()함수를 사용하여 기존의 for문을 대체가능하다.

```java
// 5이상 9이하를 갖는 Stream
IntStream stream = IntStream.range(5, 9);
```

## Stream 가공

### 필터링 - Filter

Filter는 Stream에서 조건에 맞는 데이터만을 정제하여 더 작은 컬렉션을 만들어낸다. filter 함수의 인자로 함수형 인터페이스 Predicate를 받고 있기 때문에, boolean을 반환하는 람다식을 작성하여 filter 함수를 구현할 수 있다.

다음은 '홍길동'을 포함하는 것만 반환한다.

```java
@Test
public void testCreateStream3() {
  List<String> list = Arrays.asList("홍길동", "박희영", "김종호")
  Stream<String> stream = list.stream().filter(name -> name.ntains("홍길동"));
  stream.forEach(name -> System.out.println(name));
}
```

출력결과

```
홍길동
```

### 데이터 변환 - Map

Map은 기존의 Stream 요소들을 변환하여 새로운 Stream을 만든다. 저장된 값을 특정한 형태로 변환하는데 주로 사용되며, map 함수의 인자로 함수형 인터페이스 function을 받고 있다. 예를 들어 String을 요소들로 갖는 Stream을 모두 대문자 String의 요소들로 변환하고자 할 때 map을 이용할 수 있다.

```java
@Test
public void testCreateStream4() {
  List<String> list = Arrays.asList("love", "apple", "you"); 
  Stream<String> stream = list.stream().map( s -> s.toUpperCase());
  stream.forEach(name -> System.out.println(name));
}
```

출력결과

```
LOVE
APPLE
YOU
```

### 정렬 - Sorted

Stream의 요소들을 정렬하기 위해서는 sorted를 사용한다. 파라미터로 Comparator를 넘길 수도 있다. Comparator 인자 없이 호출할 경우에는 오름차순으로 정렬이 되며, 내림차순으로 정렬하기 위해서는 Comparator의 reverseOrder를 이용한다.

```java
@Test
public void testCreateStream5() {
  List<String> list = Arrays.asList("love", "apple", "you"); 
  Stream<String> stream = list.stream().sorted(Comparator.reverseOrder());
  stream.forEach(name -> System.out.println(name));
}
```

출력결과

```
you
love
apple
```

### 중복제거 - Distinct

Stream의 요소들에 중복된 데이터가 존재하는 경우, 중복을 제거하기 위해 distinct를 사용한다. distinct는 중복된 데이터를 검사하기 위해 Object의 equals() 메소드를 사용한다.

```java
@Test
public void testCreateStream6() {
  List<String> list = Arrays.asList("love", "apple", "love", "you"); 
  Stream<String> stream = list.stream().distinct();
  stream.forEach(name -> System.out.println(name));
}
```

출력결과

```
love
apple
you
```

사용자 생성 클래스를 Stream으로 사용한다고 하면 equals와 hashCode를 오버라이드 해야만 distinct()를 제대로 적용할 수 있다.

### Peek

Stream에 영향을 주지 않고 각각의 요소들을 확인해보려면 peek을 사용한다. peek는 Stream의 각각의 요소들ㄷ에 대해 특정 작업을 수행할 뿐 결과에 영향을 미치진 않는다.

```java
  @Test
  public void testCreateStream7() {
    int sum = IntStream.of(1, 3, 5, 7, 9)
          .peek(System.out::println) 
          .sum();
  }
```

출력결과

```
1
3
5
7
9
```

### 스트림간의 변환

일반적인 Stream 객체를 원시 Stream으로 바꾸거나 그 반대로 해야하는 경우가 있다. 일반적인 Stream 객체는 mapToInt(), mapToLong(), mapToDouble()이라는 특수한 매핑 연산을 지원한다. 반대는 mapToObject()를 사용한다.

```java
@Test
public void testCreateStream8() {
  Stream<String> stream = IntStream.range(1, 5).mapToObj(i -> "a" + i);
  stream.forEach(System.out::println);
}
```

출력결과

```
a1
a2
a3
a4
```

## 결과 생성

### 최대값/최소값 등

```java
OptionalInt min = IntStream.of(1, 3, 5, 7, 9).min(); 
System.out.println(min.getAsInt());
```

출력결과

```
1
```

```java
int max = IntStream.of().max().orElse(0); 
System.out.println(max);
```

출력결과

```
0
```

```java
int max = IntStream.of(1,3,5,7,9).max().orElse(0); 
System.out.println(max);
```

출력결과

```
9
```

```java
 IntStream.of(1, 3, 5, 7, 9).average().ifPresent(System.out::println);
```

출력결과

```
5.0
```

합계나 값이 비어 있는 경우 0으로 값을 특정할 수 있다.

```java
long count = IntStream.of(1, 3, 5, 7, 9).count(); 
long sum = LongStream.of(1, 3, 5, 7, 9).sum();
System.out.println(count);
System.out.println(sum);
```

출력결과

```
5
25
```

### List로 반환하기

Stream의 요소들을 List나 Set, Map, 등 다른 종류의 결과로 수집하고 싶은 경우에는 collect 함수를 이용할 수 있다. collect 함수는 Collector 타입을 인자로 받아서 처리한다. 일반적으로 List로 Stream의 요소들을 수집하는 경우가 많은데, 이렇듯 자주 사용하는 작업은 Collectors 객체에서 static 메소드로 제공하고 있다. 원하는 것이 없는 경우에는 Collector 인터페이스를 직접 구현하여 사용할 수도 있다.

*   collect() 

    매개변수로 Collector를 필요로 함
*   Collector 

    인터페이스, collect의 파라미터는 이 인터페이스를 구현해야 함 
*   Collectors 

    클래스, static메소드로 미리 작성된 컬렉터를 제공

```java
  private static class StreamUser { 
    private String name;

    public StreamUser(String name) {
      this.name = name; 
    }
    public String getName() {
      return this.name; 
    }
    public void setName(String name) {
      this.name = name; 
    }
  };

  @Test
  public void testCreateStream11() {
    List<StreamUser> userList = Arrays.asList(
      new StreamUser("a"),
      new StreamUser("b"),
      new StreamUser("c"),
      new StreamUser("d"),
      new StreamUser("e")
    );

    List<String> nameList = userList.stream()
        .map(StreamUser::getName)
        .collect(Collectors.toList());
    nameList.forEach(System.out::println);
  }
```

출력결과

```
a
b
c
d
e
```

### 합치기 - joiniing

Stream에서 작업한 결과를 1개의 String으로 이어 붙이기를 할 때 COllector.joining()를 사용할 수 있다.

* delimiter : 구분자
* prefix : 접두사
* suffix : 접미사

```java
  @Test
  public void testCreateStream12() {

    List<StreamUser> userList = Arrays.asList(
      new StreamUser("a"),
      new StreamUser("b"),
      new StreamUser("c"),
      new StreamUser("d"),
      new StreamUser("e")
    );

    String nameString = userList.stream()
        .map(StreamUser::getName)
        .collect(Collectors.joining("/"));
    System.out.println(nameString);

  }
```

## 그룹핑 - groupingBy

Stream에서 작업한 결과를 특정 그룹으로 묶으려면Collectors.groupingBy()를 이용할 수 있다. 결과는 Map으로 반환받게 된다. groupingBy는 매개변수로 함수형 인터페이스 Function을 필요로 한다.

```java
  @Test
  public void testCreateStream12() {
    List<StreamUser> userList = Arrays.asList(
      new StreamUser("a", "가"),
      new StreamUser("b", "가"),
      new StreamUser("c", "나"),
      new StreamUser("d", "나"),
      new StreamUser("e", "다")
    );

    Map<String, List<StreamUser>> users = 
      userList.stream().collect(Collectors.groupingBy(StreamUser::getDept));
    System.out.println(users);
  }
```

출력결과

```
{가=[com.sogood.test.java.stream.StreamTest$StreamUser@1dd92fe2, 
com.sogood.test.java.stream.StreamTest$StreamUser@6b53e23f],
 다=[com.sogood.test.java.stream.StreamTest$StreamUser@64d2d351],
  나=[com.sogood.test.java.stream.StreamTest$StreamUser@1b68b9a4,
   com.sogood.test.java.stream.StreamTest$StreamUser@4f9a3314]}
```

### Boolean 값으로 그룹핑하기

Collectors.groupingBy()가 함수형 인터페이스 Function을 사용해서 특정 값을 기준으로 Stream 내의 요소들을 그룹핑하였다면, Collectors.partitioningBy()는 함수형 인터페이스 Predicate를 받아 Boolean을 Key값으로 partitioning한다.

```java
  @Test
  public void testCreateStream14() {
    List<StreamUser> userList = Arrays.asList(
      new StreamUser("a", "가"),
      new StreamUser("b", "가"),
      new StreamUser("c", "나"),
      new StreamUser("d", "나"),
      new StreamUser("e", "다")
    );

    Map<Boolean, List<StreamUser>> users = userList.stream()
         .collect(Collectors.partitioningBy(p -> p.getDept().contains("가")));
    System.out.println(users);
  }
```

출력결과

```
{false=[com.sogood.test.java.stream.StreamTest$StreamUser@6b53e23f,
 com.sogood.test.java.stream.StreamTest$StreamUser@64d2d351,
  com.sogood.test.java.stream.StreamTest$StreamUser@1b68b9a4], 
  true=[com.sogood.test.java.stream.StreamTest$StreamUser@4f9a3314, 
  com.sogood.test.java.stream.StreamTest$StreamUser@3b2c72c2]}
```

### 조건검사

요소들이 특정한 조건을 충족하는지 검사하고 싶은 경우에는 match 함수를 이용한다. match 함수는 함수형 인터페이스 Predicate를 받아서 해당 조건을 만족하는지 검사를 하게 되고, 검사 결과를 boolean으로 반환한다.

* anyMatch: 1개의 요소라도 해당 조건을 만족하는지
* allMatch: 모든 요소가 해당 조건을 만족하는지
* nonMatch: 모든 요소가 해당 조건을 만족하지 않는지

```java
  @Test
  public void testCreateStream15() {
    List<StreamUser> userList = Arrays.asList(
      new StreamUser("a", "가"),
      new StreamUser("b", "가"),
      new StreamUser("c", "나"),
      new StreamUser("d", "나"),
      new StreamUser("e", "다")
    );

    boolean anyMatch = userList.stream().anyMatch(p -> p.getName().contains("a"));
    System.out.println(anyMatch);
  }
```

출력결과

```
true
```
