# Generic

## Class 리터럴과 타입 토큰의 의미

### 클래스 리터럴

클래스 리터럴은 String.class, Integer.class 등을 말한다. String.class의 타입은 Class\\, Intger.class의 타입은 Class\이다. 클래스 리터럴은 타입 토큰으로서 사용된다.

### 타입 토큰

타입토큰은 간단히 말해 타입을 나탸내는 토큰이며 클래스 리터럴이 타입 토큰으로서 사용된다. methodA(Class\\\<?> clazz)와 같은 메서드는 타입토큰을 인자로 받는 메서드이다. jackson 라이브러리에는 readValue()라는 메서드를 사용하는데 해당 메서드는 아래와 같이 정의되어 있다. 이렇게 특정 타입의 클래스 정보를 넘겨서 타입 안정성을 가져오는 것을 Type Token이라고 한다.

```java
public <T> T readValue(String jsonString, Class<T> valueType)
```

Peson 클래스를 타입토큰으로 정의된 메소드에 파라미터로 전달하려면 다음과 같이 한다.

```java
Person person = objectMapper.readVvalue(jsonString, Person.class);
```

## WildCard

**\\\<?>** 모든 객체 자료형, 내부적으로는 Object로 인식.

**\\\<? super 객체형>** 명시된 객체자료형의 상위 객체, 내부적으로는 Object로 인식. Number 의 상위클래스만 타입으로 가지도록 하고 싶은 경우 제네릭의 타입을 제한할 수 있다.

```java
public class MyArrayList<T extends Number>
```

**\\\<? extends 객체자료형>** 명시된 객체자료형을 상속한 하위객체, 내부적으로는 명시된 객체 자료형으로 인식. Number의 서브클래스만 타입으로 가지도록 하고 싶을 때 다음과 같이 제네릭의 타입을 제한할 수 있다.

```java
public class MyArrayList<T extends Number>
```

## Class\\\<?>

어떤 종류의 클래스든 다 받아들인다는 뜻이다. 아래는 인스턴스를 통해서 타입정보를 구한다.

```java
String name = new String("latte");
Class<?> c = name.getClass();
System.out.println(c.getTypeName());
```

아래는 모든 클래스에서 내장된 class 스태틱 변수를 통해서 타입정보를 구한다.

```java
Class<?> c2 = String.class; 
System.out.println(c2.getTypeName());
```

String class를 사용하여 타입토큰이 어떻게 사용되는지 살펴보자. 다음과 같이 클래스 타입 변수를 타입토큰으로 선언할 수 있다.

```java
Class<?> c = String.class;
```

생성자를 구하는데 타입토큰을 파라미터로 넘겨준다.

```java
Class<?> parameterType = String.class; 
Constructor<?> constructor = c.getConstructor(parameterType);
```

또는 클래스 리터럴을 사용하여 넘길 수도 있다.

```java
Constructor<?> constructor = c.getConstructor(String.class);
```

문자열을 받아 들이는 생성자를 선택했기 때문에 문자열을 넘겨주어 인스턴스를 생성한다.

```java
String s = (String)constructor.newInstance("latte@gmail.com");
```

인자가 없이 getConstructor() 메서드를 선택하면 기본 생성자고 반환된다. newInstance() 메서드를 호출할 때에도 인자를 넘기지 않는다.

```java
Constructor<?> constructor = c.getConstructor();
String s = (String)constructor.newInstance();
System.out.println(s.getClass().toString());
```

## Generic Method

메소드에 제너릭을 적용하려면 리턴 타입 바로 앞에 \ 제너릭 타입을 선언해야 한다. 제너릭 타입을 배열로 넘기고 싶으면 T\[]와 같이 사용한다.

```java
public static <T> List<T> getList(T[] arr) {
}
```

전체 코드이다.

```java
public static <T> List<T> getList(T[] arr) {
  List<T> list = new ArrayList<T>();
  for(T element : arr) {
    list.add(element); 
  }
  return list; 
}//:
```

다음과 같이 사용할 수 있다.

```java
String[] arr = new String[] { "Apple", "Pear"};
List<String> list = GenericTestBean.getList(arr); 
list.forEach( element -> {
  System.out.println(element);
});
```

## 슈퍼타입토큰

### 타입 파라미터

다음과 같이 Generic 클래스를 만든다.

```java
public class Generic<T> {
}
```

이것을 다음과 같이 사용할 수 있다.

```java
MyGeneric<String> gen = new MyGeneric<>();
```

위의 코드의 MyGeneric\에서 String을 '타입 파라미터'라고 한다.

### 강제 캐스팅

강제 캐스팅은 컴파일내에는 오류를 발생시키지 않지만, 런타임시에 오류를 만들 수 있으므로 위험하다. 즉, 아래 코드와 같은 위험성이 있다.

```java
Object o = "I love you.";
Integer i = (Integer)o;
```

### 타입에 안전한 Map 작성

아래와 같이 Map을 작성하면 타입에 안전할 것 같지만 get() 메서드에서 value를 강제로 타입을 캐스팅하기 때문에 위험하다.

```java
static class MySafeMap {
      Map<Class<?>, Object> map = new HashMap<>();
      <T> void put(Class<T> clazz, T value) {
          map.put(clazz, value);
      }
      <T> T get(Class<T> clazz) {
          return (T) map.get(clazz);
      }
}
```

강제 캐스팅이 없는 Map을 다음과 같이 작성한다. Class 타입이 제공하는 cast 를 사용한다.

```java
static class MySafeMap {
  Map<Class<?>, Object> map = new HashMap<>();
  <T> void put(Class<T> clazz, T value) {
      map.put(clazz, value);
   }
  <T> T get(Class<T> clazz) {
    return clazz.cast(map.get(clazz)); /// class의 cast() 사용하여 안전한 캐스팅
  }
}
```

위에서 작성한 타입에 안전한 맵에 List\\, List\을 추가하려고 하면 오류가 발생하여 추가할 수 없다. List\\.class라는 클래스 리터럴이 없기 때문이다. 아래 코드는 오류가 발생한다.

```java
TypeSafeMap m = new TypeSafeMap();
m.put(List<String>.class, Arrays.asList("aa", "bb", "cc")); // 오류가 발생한다.
```

### 해결방법

다음과 같이 MySuper 클래스를 정의해 보자.

```java
static class MySuper<T> {
  T value; 
  T get() { return value; }
  void set(T t) {value = t; }
}
```

다음과 같이 MySuper 클래스에 String 타입 파라미터를 사용하여 인스턴스화하고 value의 타입을 알아보자.

```java
MySuper<String> mysup = new MySuper<>();
System.out.println(mysup.getClass().getDeclaredField("value").getType());
```

결과는 java.lang.String이 아닌 java.lang.Objet가 출력된다. 이유는 type erasure가 타입 파라미터의 정볼를 런타임 시에 지워버리기 때문이다. 이것은 Java5 이상의 컴파일러도 이전 버전의 코드를 문제없이 컴파일하기 위해서 컴파일 시에 제네릭 정보들을 소거시키기 때문이다. 이것을 TypeErasure이라고 한다.

하지만 새로운 타입을 정의하면서 슈퍼 클래스를 Generic 타입으로 타입을 지정하여 상속하면 런타임 시에도 정보가 남아 있기 때문에 다음의 코드와 같이 작성하면 정보를 구할 수 있다.

```java
static class MySuper<T> {
  T value; 
  T get() { return value; }
  void set(T t) {value = t; }
}
static class MySubclass extends MySuper<String> {
}
```

getGenericSuperclass()로 타입을 구한다.

```java
MySubclass mysub = new MySubclass();
Type t = mysub.getClass().getGenericSuperclass();
ParameterizedType ptype = (ParameterizedType) t;
System.out.println(ptype.getActualTypeArguments()[0]);
```

결과값은 java.lang.String이 출력된다.

이것을 로컬 클래스로 만들어서 타입 정보를 가져올 수 있다.

```java
@Test
public void testSafeMap7() throws Exception {

  class MySubclass2 extends MySuper<List<String>> {
  }
  MySubclass2 mysub = new MySubclass2();
  Type t = mysub.getClass().getGenericSuperclass();
  ParameterizedType ptype = (ParameterizedType) t;
  System.out.println(ptype.getActualTypeArguments()[0]);
}
```

결과 값은 다음과 같다.

```
java.util.List<java.lang.String>
```

다시, 익명 Local 클래스로 만들어서 타입정보를 가져올 수 있다.

```java
@Test
public void testSafeMap8() throws Exception {
  MySuper<List<String>> mysuper =  new MySuper<>() {}; // 로컬 익명 클래스
  Type t = mysuper.getClass().getGenericSuperclass();
  ParameterizedType ptype = (ParameterizedType) t;
  System.out.println(ptype.getActualTypeArguments()[0]);
}
```

결과 값은 앞의 결과와 동일하게 출력된다.

```
java.util.List<java.lang.String>
```

지금까지 설명한 것과 같이 제너릭을 상속하여 익명클래스로 만들어 타입의 정보를 가져오는 방식을 슈퍼타입토큰 방식이라고 한다.

### Jackson 라이브리의 TypeReference

이제 Jackson 라이브러리의 TypeReference의 사용법을 이해할 수 있을 것이다.

```java
map = mapper.readValue(json, new TypeReference<Map<String, String>>(){});
```
