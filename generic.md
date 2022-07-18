# Generic


제네릭(Generic)은 클래스 내부에서 지정하는 것이 아닌 외부에서 사용자에 의해 지정되는 것을 의미한다. 한마디로 특정(Specific) 타입을 미리 지정해주는 것이 아닌 필요에 의해 지정할 수 있도록 하는 일반(Generic) 타입이라는 것이다.

아래 표와 같은 타입들이 많이 사용된다. 

|타입	|설명|
|----|-----|
|\<T\>|	Type|
|\<E\>	|Element|
|\<K\>|	Key|
|\<V\>	|Value|
|\<N\>	|Number|



## 제네릭 클래스
```java
public class ClassName <T> { ... }
```

>  T 타입은 해당 코드 블럭 { } 안에서 유효하다. 


Generic Type을 여러 개 둘 수 있다. 
```java
public class ClassName <T, K> { ... }
```
>  타입 파라미터로 명시할 수 있는 것은 참조 타입(Reference Type)밖에 올 수 없다. 즉, int, double, char 같은 primitive type은 올 수 없다. 


### 간단한 Generic 클래스 
```java
 class MyClass<K, V> {	
    private K first;	// K 타입(제네릭)
    private V second;	// V 타입(제네릭) 
    
    void set(K first, V second) {
      this.first = first;
      this.second = second;
    }
    
    K getFirst() {
      return first;
    }
    
    V getSecond() {
      return second;
    }
  }

```
다음과 같이 사용할 수 있다. 
```java
    MyClass<String, String> myClass = new MyClass<String, String>();
    myClass.set("Hong", "Park");
    String first = myClass.getFirst();
    String second = myClass.getSecond();
    System.out.println(first);
    System.out.println(second);
```



### 제네릭 메소드 


제네릭은 외부에서 유형을 지정해 준다. 해당 클래스가 인스턴스화 했을 때, new 생성자로 클래스를 생성하고 \< \> 괄호 사이에 파라미터로 넘겨준 타입으로 저정된다. 하지만 정적 메소드는 클래스가 인스턴스화 하기 전에 프로그램이 실행 시에 이미 메모리에 올라간다. static 변수, static 메소드 등 static이 붙은 것들은 프로그램 실행 시에 메모리에 이미 올라간다는 의미다. 

따라서 static method에서 제네릭을 사용할 때에는 타입 정보를 얻어 올 곳이 없기 때문에 클래스와는 별도로 독립적인 제네릭이 사용되어야 한다. 


- 제네릭 타입과 다르게 제네릭 메소드는 **타입 파라미터의 사용범위가 메소드 내로 한정**
- 만약 제네릭 타입에 `T`라는 메소드 파라미터를 선언하더라도 제네릭 메소드에 타입 파라미터 `T`를 선언하면 제네릭 메소드의 타입 파라미터 `T`에 **우선순위를 둔다.**

**형식**
```java
//  ①<T> generic type, ②T return type,  ③T parameter type
public static ① <T> ②T getName(③T name) { return name; }
```


```java
ppublic static <K, V> boolean methodName(K key, V value) {
    return true;
}
```

- 우선, 타입 파라미터의 리스트를 선언 해야 한다. <K, V>가 선언 부분이다.
- 선언 부분은 반드시 리턴 타입 앞에 와야 한다.
- 타입 파라미터로 메소드의 인자 `key`, `value` 를 각각 받고 있다.

**예시**
```java
  class MyClass<K, V> {	
    private K first;	// K 타입(제네릭)
    private V second;	// V 타입(제네릭) 
    
    void set(K first, V second) {
      this.first = first;
      this.second = second;
    }
    
    K getFirst() {
      return first;
    }
    
    V getSecond() {
      return second;
    }
    /** 제네릭 메소드의 K 타입은 클래스의 K 타입과는 다른 타입이다. */
    static <K> K genericMethod(K o) {
      return o; 
    }
  }
```

```java
  @Test
  public void testGenericClass() {
    MyClass<String, String> myClass = new MyClass<String, String>();
    myClass.set("Hong", "Park");
    String first = myClass.getFirst();
    String second = myClass.getSecond();
    System.out.println(first);
    System.out.println(second);

    // Generic Method 사용 
    String jeong = MyClass.genericMethod("Jeong");
    System.out.println(jeong);

  }
```

메소드에 제너릭을 적용하려면 리턴 타입 바로 앞에  제너릭 타입을 선언해야 한다. 제너릭 타입을 배열로 넘기고 싶으면 T\[]와 같이 사용한다.

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


## 제네릭 인터페이스

```java
public Interface InterfaceName <T> { ... }
```

>  T 타입은 해당 코드 블럭 { } 안에서 유효하다. 

Generic Type을 여러 개 둘 수 있다. 
```java
public Interface InterfaceName <T, K> { ... }
```
>  타입 파라미터로 명시할 수 있는 것은 참조 타입(Reference Type)밖에 올 수 없다. 즉, int, double, char 같은 primitive type은 올 수 없다. 


### 간단한 Generic Interface
```java
  interface MyInterface<T, K> {
    T doSomething(T t);
    K doSomething2(K k);
  }
```
### Generic interface를 구현한 Generic class
 제네릭 인터페이스를 구현하는 제네릭 클래스를 만든다. 클래스 정의는 동일한 형식 매개변수를 두 번 사용한다. 하나는 클래스 이름 뒤이고 다른 하나는 구현하는 인터페이스 이름 뒤에 있다.  다음 코드를 고려하라.  두 위치에서 \<T,K\>를 두 번 사용 합니다.


```java
  // Generic Interface를 상속한 Generic class
  static class MyClass<T, K> implements MyInterface<T, K> {
    public T doSomething(T t) {
      return t; 
    }
    public K  doSomething2(K k) {
      return k;
    }
  }
```
사용하기 
```java
  @Test 
  public void testGenericInterfaceA() {
    // Generic class 생성 
    MyClass<String, Integer> myclass = new MyClass<String, Integer>(); 
    String t = myclass.doSomething("Hello");
    Integer k = myclass.doSomething2(1);
    System.out.println(t);
    System.out.println(k);
  }
```

### Generice interface를 구현한 Non-Generic class
형식 매개 변수를 사용하는 대신 실제 형식을 사용한다. 예를 들어 \<T, K\> 대신 \<String, Integer\>과 같이 사용한다. 
```java

  // Generic Interface를 상속한 Non Generic class 
  // Type을 구체적으로 직접 지정해야 한다. 
  class MyNonClass implements MyInterface<String, Integer> {
    // 구체적으로 타입을 명시해야 
    public String doSomething(String t) {
      return t; 
    }
    // 구체적으로 타입을 명시해야 
    public Integer  doSomething2(Integer k) {
      return k;
    }
  }
```

사용하기 
```java
  @Test 
  public void testGenericInterfaceB() {
    // Generic class 생성 
    MyNonClass myclass = new MyNonClass(); 
    String t = myclass.doSomething("Hello2");
    Integer k = myclass.doSomething2(2);
    System.out.println(t);
    System.out.println(k);
  }

```


## 와일드 카드 타입 

제네릭 타입을 매개값이나 리턴 타입으로 사용할 때 구체적인 타입 대신 와일드 카드를 다음과 같이 세 가지 형태로 사용할 수 있다. 

* \<?\>
* \<? extends 상위타입 \>
* \<? super 하위타입 \>


### \<?\>
* 모든 타입이 올 수 있다. 

아래는 모든 타입을 허용한다.
```java
 public Stuedent getStudent(Course<?> course) {
   // 
 }
```


### \<? extends T\>

* 상위 클래스를 제한한다.
* 타입 파라미터를 대치하는 구체적인 타입으로 상위 타입이나 하위 타입이 올 수 있다. 

다음은 Student와 Student의 하위 타입만 올 수 있다. 

```java
 class MyClass<T extends Student> {
 }
```

### \<? super T\>
추상적인 방향으로의 타입 변환을 허용한다. 

* 자기 자신과 부모 객체만 허용한다. 
* 타입 파라미터를 대치하는 구체적인 타입으로 하위 타입이나 상위타입이 올 수 있다. 


다음은 Student와 부모 타입만 올 수 있다.   
```java
class MyClass<T supser Student> {
}
```



## Class 리터럴과 타입 토큰의 의미

### 클래스 리터럴


클래스 리터럴은 String.class, Integer.class 등을 말한다. String.class의 타입은 Class, Intger.class의 타입은 Class이다. 클래스 리터럴은 타입 토큰으로서 사용된다.

*  클래스 리터럴은 타입 토큰으로서 사용된다.
* **String.class, Integer.class 등을 말한다**



### 타입 토큰

타입토큰은 간단히 말해 타입을 나탸내는 토큰이며 클래스 리터럴이 타입 토큰으로서 사용된다. methodA(Class\<?\> clazz)와 같은 메서드는 타입토큰을 인자로 받는 메서드이다. jackson 라이브러리에는 readValue()라는 메서드를 사용하는데 해당 메서드는 아래와 같이 정의되어 있다. 이렇게 특정 타입의 클래스 정보를 넘겨서 타입 안정성을 가져오는 것을 Type Token이라고 한다.


```java
public <T> T readValue(String jsonString, Class<T> valueType)
```

Peson 클래스를 타입토큰으로 정의된 메소드에 파라미터로 전달하려면 다음과 같이 한다.

```java
Person person = objectMapper.readVvalue(jsonString, Person.class);
```


## Class\<?\>

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

**Class 정보를 얻는 방법**     

```java

    // 인스턴스를 통해서 얻기
    // 인스턴스만 있으면 그 인스턴스가 어떤 클래스의 객체인지 쉽게 알아낼 수 있다.
    String obj = new String("jirepos");
    Class<?> c1 = obj.getClass(); // <?> : 어떤 종류의 클래스든 다 얻어올 수 있다
    
    
    // class.forName() 메서드를 통해서 얻기
    // 인자가 가리키는 문자열이 없을 수 있기 때문에 예외처리가 필요
    // 클래스 이름을 문자열로 받을 수 있기 때문에 유지보수가 쉽다.
    Class<?> c2 = Class.forName("java.lang.String"); 
        
    // 모든 클래스에 내장된 "class" 스태틱 변수를 통해서 얻기
    // 클래스 이름을 코드로 명시하기 때문에 유지보수가 어렵다.
    Class<?> c3 = String.class;
    

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



## Multiple Bounds
Multiple Bounds는 유형 변수는 경계에 나열된 모든 유형의 하위 유형이다. 경계 중 하나가 클래스이면 먼저 지정해야 한다.

```java
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }
```

다음은 오류가 발생한다. 
```java
class D <T extends B & A & C> { /* ... */ }  // compile-time error
```

**예시**
다중 바인딩을 사용하려면 & 연산자를 사용하는데  CodeEnum은 인터페이스이어야 한다. 
```java
  public static class MyHandler<E extends Enum<E> & CodeEnum<String>> {
    private EnumSet<E> enumSet;

    public MyHandler(Class<E> type) {
      this.enumSet = EnumSet.allOf(type);
    }

    public E getEnum(String code) {
      return this.enumSet.stream().filter(constant -> {
        // CodeEnum<T> 인터페이스에 정의된 getCode()를 직접 호출 할 수 있다. 
        return constant.getCode().equals(code) ? true : false;
      }).findFirst().orElseGet(() -> null);
    }
  }
```  

