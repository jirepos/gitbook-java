# Optional

비어있는 Optional 객체를 얻기 위해서 empty()를 사용한다.

```java
Optional<String> opt1 = Optional.empty();
System.out.println(opt1);
```

출력결과

```
Optional.empty
```

Optional 안의 객체가 비어있는지 체크하기 위해서 empty()를 사용하고 존재하는지 체크하려면 isPresent()를 사용한다.

```java
Optional<String> opt1 = Optional.empty();
if(opt1.isPresent()) {
   System.out.println("Present");
}else {
  System.out.println("No Present");
}
```

출력결과

```
No Present
```

Optional에 null을 할당하고 싶으면 ofNullable()를 사용한다.

```java
Optional<List<String>> opt2 = Optional.ofNullable(null);
if(opt2.isEmpty()) {
  System.out.println("empty");
}
```

출력결과

```
empty
```

Null이 아닌 객체를 할당하려면 of()를 사용한다. null을 할당하면 NPE(NullPointerException)이 발생한다. 아래 코드를 실행하면 NPE가 발생한다.

```java
Optional<List<String>> opt2 = Optional.of(null);
if(opt2.isEmpty()) {
  System.out.println("empty");
}
```

Optional 내부의 할당된 객체를 가져오려면 get()를 사용한다.

```java
Optional<String> opt = Optional.of("Hong");
String val = opt.get();
System.out.println(val);
```

출력결과

```
Hong
```

내부 객체가 비어 있을 경우 다른 값을 가져오게 하려면 orElse()를 사용한다.

```java
Optional<String> opt = Optional.ofNullable(null);
String val = opt.orElse("other");
System.out.println(val);
```

출력결과

```
other
```

orElseGet()를 사용할 수 있는데 orElse는 값을 전달하는 반면에 orElseGet()은 Supplier 인터페이스 구현체를 전달한다.

```java
@Test 
public void testOptional6() { 
  Optional<String> opt = Optional.ofNullable(null);
  String val = opt.orElseGet(() -> "Hello");
  System.out.println(val);
}//:
```

> orElse는 null이던 아니던 항상 호출된다. orElseGet은 null일 때만 호출된다.
