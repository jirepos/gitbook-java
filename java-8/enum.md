# Enum

Enum의 각 열거형 상수에 추가 속성을 정의할 수 있는데 문자열로 정의할 수 있다. 문자열 뿐만 아니라 int, double 등도 가능하다. 이럴 때 아래 코드와 같이 작성한다.

* Enum은 new를 사용하여 생성 할 수 없습니다.
* Enum 인스턴스는 코드 상에서 처음 호출되거나 참조 될 때 생성된다.
* 문자열 값을 담을 문자열 변수를 private로 선언한다. 
* 문자열 하나를 파라미터로 취하는 생성자를 private로 선언한다. 
*   Enum 인스턴스의 현재 값을 반환하는 메서드를 하나 정의한다. 

    > 생성자는 private 이어야한다.

```java
  public enum Lang {

    C("C"), JAVA("Java"), GO("Go");
    private String value;
    private Lang(String value) {
      this.value = value; 
    }
    public String value() {
      return this.value; 
    }
  }
```

Enum 객체 생성 아래 코드와 같이 열거형 상수를 사용하여 Enum 객체를 생성할 수 있다.

```java
Lang lang = Lang.JAVA;
```

생성된 객체의 열거형 상수의 이름을 가져와 보자. name() 메서드를 사용한다.

```java
System.out.println(lang.name());
```

'JAVA'가 출력될 것이다.

valueOf("상수이름")을 사용한 객체 생성 valueOf("상수이름")을 사용하여 객체를 생성할 수 있다.

```java
Lang lang2 = Lang.valueOf("GO");
System.out.println(lang2.name());
```

'GO'가 출력될 것이다.

value()를 사용한 속성 값 출력 위에서 GO 열거형 상수에 문자열 속성을 추가하면서 값으로 "Go"를 설정했었다. value() 메서드를 사용하여 값을 출력할 수 있다.

```java
System.out.println(lang2.value());
```

'Go'가 출력될 것이다.

아래와 같이 GO 열거형 상수의 문자열 속성의 값을 변경해 보자.

```java
C("C"), JAVA("Java"), GO("Other");
```

다시 값을 출력해 보자.

```java
Lang lang2 = Lang.valueOf("GO");
System.out.println(lang2.name());
System.out.println(lang2.value());
```

name()은 'GO'가 출력되고 value()는 'Other'가 출력될 것이다.

switch 구문에서 사용 Enum은 switch 구문에서 간단히 사용할 수 있다.

```java
    Lang lang = Lang.JAVA;   // Enum 선언
    switch(lang) {
      case JAVA:
      System.out.println("Java");
      break;
      default:
      System.out.println("default");
      break;
    }
```

'Java'가 출력될 것이다.

Enum 비교 == 연산자를 사용하여 간단히 비교할 수 있다.

```java
Lang lang = Lang.JAVA;   // Enum 선언
if(lang == Lang.JAVA ){ 
  System.out.println("Lang is java");
}else  { 
  System.out.println("Not equal");
}
```

'Lang is Java'가 출력될 것이다.

두 개의 속성을 사용하는 Enum 숫자와 문자열 추가 속성을 가지는 enum을 정의해 보자. 전체코드는 다음과 같다.

```java
public enum Lang {
  C(1, "C"), JAVA(2, "Java"), GO(3, "Other");

  private int langCode;
  private String langName; 
  public int getLangCode() {
    return this.langCode;
  }
  public String getLangName() {
    return this.langName; 
  }
  private Lang(int langCode, String langName) {
    this.langCode = langCode; 
    this.langName = langName; 
  }
}
```

추가 속성은 다음과 같이 정의한다.

```java
C(1, "C"), JAVA(2, "Java"), GO(3, "Other");
```

첫번재 추가 속성과 두번째 추가 속성에 대해서 변수를 선언하고 getter를 정의한다.

```java
  private int langCode;
  private String langName; 
  public int getLangCode() {
    return this.langCode;
  }
  public String getLangName() {
    return this.langName; 
  }
```

생성자는 추가 속성 순서대로 파라미터를 정의한다.

```java
  private Lang(int langCode, String langName) {
    this.langCode = langCode; 
    this.langName = langName; 
  }
```
