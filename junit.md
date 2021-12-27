# JUnit Test 


## 설치 
Junit 5 중요한 차이점은 다음과 같다. 

* JUnit 4가 단일 jar였던 것에 비해, JUnit 5는 크게 JUnit Platform, JUnit Jupiter, JUnit Vintage 모듈로 구성되어 있다.
* JUnit Jupiter는 테스트 코드 작성에 필요한 junit-jupiter-api 모듈과 테스트 실행을 위한 junit-jupiter-engine 모듈로 분리되어 있다.
* Java 8 이상 

```xml
      <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.7.1</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <version>5.7.1</version>
        <scope>test</scope>
    </dependency>
```

> 기존에 JUnit 4 버전으로 작성한 테스트 코드를 실행할 때에는 vintage-engine 모듈을 사용한다.





## Tests 작성 
다음은 최소한의 간단한 테스트 케이스를 볼 수 있다. @Test 어노테이션을 사용한다. 
```java
import org.junit.jupiter.api.Test;

public class JunitExpTest {
  @Test
  public void testFirst(){ 
    System.out.println("This is the first test case");
  }
}//~
```

### Test 실행 전에 조건 설정하기
@BeforeEach 어노테이션을 사용하면  @Test, @RepeatedTest, @ParameterizedTest, or @TestFactory 어노테이션이 붙은 테스트 메소드 실행 전에 실행된다. 


```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class JunitExpTest {
  @BeforeEach 
  void init() {
    System.out.println("This should be executed before some tests.");
  }
  @Test
  public void testFirst(){ 
    System.out.println("This is the first test case");
  }
}//~
```
실행결과는 다음과 같다. 
```shell
This should be executed before some tests.
This is the first test case
```
테스트 케이스를 하나 더 정의해 보자. 
```java
  @Test
  public void testSecond(){ 
    System.out.println("This is the second test case");
  }
```

testSecond() 테스트트를 실행하면 @BeforeEach 어노테이션을 붙인 메소드가 실행된 것을 확인할 수 있다. 
```shell
This should be executed before some tests.
This is the second test case
```

### Test 실행 후 조건 해제 하기 
@AfterEach를 사용하면 @Test, @RepeatedTest, @ParameterizedTest, or @TestFactory method 테스트를 실행한 후에 실행할 수 있다. 

```java
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class JunitExpTest {

  @BeforeEach 
  void init() {
    System.out.println("This should be executed before some tests.");
  }

  @Test
  public void testFirst(){ 
    System.out.println("This is the first test case");
  }
  
  @AfterEach 
  void tearDown() {
    System.out.println("This is executed after some tests");
  }

}//~
```
testFirst() 테스트를 실행하면 @AfterEach가 붙은 tearDown() 메소드가 실행된 것을 확인할 수 있다. 
```shell
This should be executed before some tests.
This is the first test case
This is executed after some tests
```


### 테스트 케이스 이름 
@DisplayName을 사용하여 테스트 클래스나 메소드에 대한 이름을 표시할 수 있다. 

```java

import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("Junit 사용방법 테스트 케이스")
public class JunitExpTest {
  @Test
  @DisplayName("첫번재 테스트 케이스")
  public void testFirst(){ 
    System.out.println("This is the first test case");
  }
}//~
```

### @BeforeAll, @AfterAll
@BeforeAll은 모든 테스트 케이스들이 실행전에 실행이 된다. @AfterAll은 모든 테스트 케이스들이 실행이 된 후에 실행이 된다.

모든 테스트 케이스들을 실행하기 전에 한 번만 초기화를 하고 모든 테스트 케이스들이 실행 된 이후에 가비지를 청소하기위해 사용한다. 

> static method로 선언되어야 한다. 


```java
package com.sogood.test.junit;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("Junit 사용방법 테스트 케이스")
public class JunitExpTest {

  @BeforeAll
  static void initAll() {
    System.out.println("This should be executed once before all tests");
  }

  @BeforeEach 
  void init() {
    System.out.println("This should be executed before some tests.");
  }


  @Test
  @DisplayName("첫번재 테스트 케이스")
  public void testFirst(){ 
    System.out.println("This is the first test case");
  }

  @Test
  public void testSecond(){ 
    System.out.println("This is the second test case");
  }
  
  @AfterEach 
  void tearDown() {
    System.out.println("This is executed after some tests");
  }
    
  @AfterAll
  static void tearDownAll() {
    System.out.println("This is executed once after all tests");
  }

}//~
```
실행 결과를 보면 처음과 끝에 한 번 실행된 것을 확인할 수 있다. 
```shell
This should be executed once before all tests
This should be executed before some tests.
This is the first test case
This is executed after some tests
This should be executed before some tests.
This is the second test case
This is executed after some tests
This is executed once after all tests
```

### 파라미터를 전달하여 테스트 
@ParameterizedTest 어노테이션을 사용하면 파라미터를 전달하여 여러번 테스트 할 수 있다. 
```java
package com.sogood.test.junit;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

public class JunitExpTest {
  @ParameterizedTest
  @ValueSource(strings = { "hello", "I", "am"})
  void testParam(String text) {
    System.out.println(text);
  }
}//~
```
실행된 결과는 다음과 같다. 
```shell
hello
I
am
```


허용되는 타입은 다음과 같다. 
```java
package org.junit.jupiter.params.provider;

public abstract @interface ValueSource {
  public abstract short[] shorts() default {};
  public abstract byte[] bytes() default {};
  public abstract int[] ints() default {};
  public abstract long[] longs() default {};
  public abstract float[] floats() default {};
  public abstract double[] doubles() default {};
  public abstract char[] chars() default {};
  public abstract boolean[] booleans() default {};
  public abstract java.lang.String[] strings() default {};
  public abstract  java.lang.Class<?>[] classes() default {};
}
```

**null 값 허용** 
@ValueSource 주석은 null 값을 허용하지 않는다.  @NullSource를 를 사용한다. 
```java
import org.junit.jupiter.params.provider.NullSource;

  @ParameterizedTest
  @NullSource 
  @ValueSource(strings = { "hello", "I", "am"})
  void testparamNull(String text) {
    System.out.println(text);
  }
```

**enum 사용** 
enum 클래스를 간단히 생성한다. 
> Inner class는 동작이 안된다. 별개의 파일로 생성해야 한다. 


```java
public enum HttpProtocol {
  HTTP1, HTTP2;
}
```
@EnumSource를 사용한다. 
```java
  @ParameterizedTest
  @EnumSource(HttpProtocol.class) 
  void testEnumSource(HttpProtocol protocol) {
    System.out.println(protocol.name());
  }
```  



**CSV 사용** 
전달하려고 하는 데이터 많은 경우에 @CsvSource를 사용하면 유용하다. 
```java
  @ParameterizedTest
  @CsvSource( {
    "홍길동, 10, 2020-12-20",
    "허난설, 20, 2021-01-20",
    "박한별, 25, 2019-12-01",
  })
  void testCSV(String name, int age, LocalDate date) {
    System.out.printf("%s, %d, %s", name, age, date);
    System.out.println("");
  }
```
출력 결과는 다음과 같다. 
```shell
홍길동, 10, 2020-12-20
허난설, 20, 2021-01-20
박한별, 25, 2019-12-01
```




## Assertions 

Assertion이 실패하면 Test Case가 실패한다. 

### 두 값이 같은지 비교 
```java
// assertEquals(기대값, 실제값)
    assertEquals(2,2);
    assertNotEquals(2, 3);
```

### 값이 참인지 검증 
```java
    assertTrue( 10 > 5);
    assertFalse(10 < 5);
```    
### Null인지 확인
```java
    Object dog = new Object();
    assertNotNull(dog, () -> "The dog should not be null");
    
    Object cat = null;
    assertNull(cat, () -> "The cat should be null");
  }
```
### 무조건 실패
```java
  /** fail Test  */
  @Test
  void testFail() {
    // 무조건 실패
    fail("Failed");
  }
```
### 여러개의 Assertion 묶어서 실행
```java
  /** assertAll Test */
  @Test
  void testAssertAll() {
    // 여러개의 assertion을 묶어서 실행 
    assertAll(
      "heading",
      () -> assertEquals(4, 2 * 2, "4 is 2 times 2"),
      () -> assertEquals("java", "JAVA".toLowerCase())
    );
  }
```










## References
[Use Guide](https://junit.org/junit5/docs/current/user-guide/)
[Parameterized Test](https://www.arhohuttunen.com/junit-5-parameterized-tests/)


