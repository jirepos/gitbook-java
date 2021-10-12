# Functional Interface

Java SDK 8의 java.util.function 패키지에는 수많은 Functional Interface들이 등록되어 있다. 이 패키지에 등록되어 있는 모든 인터페이스들은 @FunctionalInterface로 지정되어 있다. 람다식이나 메서드 레퍼런스를 위한 할당 대상으로 사용될 수 있다.

아래 코드에서 간단히 Functional Interface를 정의하고 사용하는지 살펴보자.

```java
package com.sogood.test.java.func;

import org.junit.jupiter.api.Test;

public class FunctionalTest {
  /* 함수형 인터페이스. 1개의 추상 메소드를 갖는다. */ 
  static interface FunctionalInterface {
    public abstract void doSomething(String text);
  }

  /** 함수형 인터페이스 사용 */
  @Test 
  public void testFunctional(){ 
    //함수형 인터페이스를 사용하는 이유는 자바의 람다식은 함수형 인터페이스로만 접근이 되기 때문.
    FunctionalInterface func = text -> System.out.println(text); 
    func.doSomething("hello");
  }
}///~
```

> Functional Interface는 메서드가 하나만 존재한다.

## @FunctionalInterface

Java SDK 8에서는 @FunctionalInterface이라는 어노테이션을 제공하여 작성한 인터페이스가 Fucntional Interface인지 확인할 수 있도록 하고 있는데 FucntionalInterface어노테이션은 인터페이스가 Fucntional Interface인지 아닌지 확실히 하기를 원할 때 사용하면 된다.

@FunctionalInteface를 사용하여 interface를 정의한다.

```java
package com.sogood.test.java.func;

@FunctionalInterface
public interface MyFuncInterface {
  void print(String s);
}
```

다음과 같이 Lamba식을 사용하여 메소드를 정의하고 사용할 수 있다.

```java
@Test 
public void testMyFunctional(){ 
  MyFuncInterface func = text -> System.out.println(text); 
  func.print("hello");
}
```

## Java가 제공한는 기본 Interface

*   Consumer    

    void accept(T) 메서드가 선언되어 있는 인터페이스. 입력된 T type 데이터에 대해 어떤 작업을 수행하고자 할 때 사용한다. 리턴 타입이 void이므로 처리 결과를 리턴해야 하는 경우에는 Function 인터페이스를 사용해야 한다. 혹은 call by reference를 이용하여 입력된 데이터의 내부 프러퍼티를 변경할 수도 있다. 
*   Function        

    R apply(T) 메서드가 선언되어 있는 인터페이스. 입력된 T type 데이터에 대해 일련의 작업을 수행하고 R type 데이터를 리턴할 때 사용한다. 입력된 데이터를 변환할 때 사용할 수 있다. 
* Predicate boolean test(T) 메소드가 선언되어 있는 인터페이스. 입력된 T type 데이터가 특정 조건에 부합되는지 확인하여 boolean 결과를 리턴한다.
* Supplier\
  T get() 메서드가 선언되어있는 인터페이스이다. 매개변수를 받지 않고 특정 타입의 결과를 리턴한다.

### Consumer

리턴을 하지않고(void), 인자를 받는 메서드를 갖고있다. 인자를 받아 소모한다는 뜻으로 인터페이스 명칭을 이해하면 될듯 하다.

```java
Consumer<String> c = str -> System.out.println(str);
c.accept("hello consumer");
```

### Function

인터페이스 명칭에서부터 알 수 있듯이 전형적인 함수를 지원한다고 볼 수 있다. 하나의 인자와 리턴타입을 가지며 그걸 제네릭으로 지정해줄수있다. 그래서 타입파라미터(Type Parameter)가 2개다.

```java
@Test
public void testFunction() {
  Function<String, Integer> f = str -> Integer.parseInt(str);
  Integer result = f.apply("1");
  System.out.println(result);
}
```

### Supplier

인자는 받지않으며 리턴타입만 존재하는 메서드를 갖고있다. 순수함수에서 결과를 바꾸는건 오직 인풋(input) 뿐이다. 그런데 인풋이 없다는건 내부에서 랜덤함수같은것을 쓰는게 아닌이상 항상 같은 것을 리턴하는 메서드라는걸 알 수 있다.

```java
@Test
public void testSupplier() {
  Supplier<String> s = () -> "hello supplier";
  String result = s.get();
  System.out.println(result);
}
```
