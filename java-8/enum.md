# Enum

Enum의 각 열거형 상수에 추가 속성을 정의할 수 있는데 문자열로 정의할 수 있다. 문자열 뿐만 아니라 int, double 등도 가능하다. 이럴 때 아래 코드와 같이 작성한다.

* Enum은 new를 사용하여 생성 할 수 없습니다.
* Enum 인스턴스는 코드 상에서 처음 호출되거나 참조 될 때 생성된다.
* 문자열 값을 담을 문자열 변수를 private로 선언한다. 
* 문자열 하나를 파라미터로 취하는 생성자를 private로 선언한다. 
*   Enum 인스턴스의 현재 값을 반환하는 메서드를 하나 정의한다. 

    > 생성자는 private 이어야한다.


## 간단한 Enum 

간단히 이름만 나열하여 ENUM을 정의할 수 있다. 

```java
  public enum ServerEnum {
    MOBILE, WEB, SERVER
  }
```
다음과 같이 Enum의 인스턴스를 생성한다.
```java
   // enum 객체 생성 
    ServerEnum server = ServerEnum.MOBILE;
```
System.out으로 출력하면 ENUM 이름이 출력된다. 
```java
    // enum 객체 생성 
    ServerEnum server = ServerEnum.MOBILE;
    System.out.println(server);  // MOBILE
```

### name() 
Enum의 이름은 name() 메서드를 사용하여 출력할 수 있다. 
```java
System.out.println(server.name());  // MOBILE
```

### valueOf()
Enum의 이름으로 Enum의 인스턴스를 valueOf(String) 메서드를 사용하여 생성할 수 있다. 
```java
    ServerEnum serverType = ServerEnum.valueOf("MOBILE");
    System.out.println(serverType.name());  // MOBILE
```

### ordinal() 
ordinal() 메소드는 해당 열거체 상수가 열거체 정의에서 정의된 순서(0부터 시작)를 반환한다.  반환 값은 정의된 숫서를 의미하며 상수 값이 아니다. 
```java
    ServerEnum serverType = ServerEnum.valueOf("MOBILE");
    System.out.println(serverType.ordinal()); // 0
```


### values() 
해당 열거체의 모든 상수를 선언된 순서대로 배열을 생성하여 반환한다. 
```java
    for(ServerEnum ele: ServerEnum.values()) {
      System.out.println(ele);
      // MOBILE
      // WEB 
      // SERVER
    }
```

## 초기 값을 가지는 enum 
### 정수 초기값을 가지는 enum
Enum 상수에 초기값을 명시할 수 있다.  값의 타입에 따라 변수를 생성하고 getter를 생성해야 한다. 생성자는 private 이어야 한다. 

```java
  enum Season{  
    WINTER(10),SUMMER(20);  

    private int value;  
    public int value() {
      return this.value; 
    }

    Season(int value){  
      this.value=value;  
    }  
  }      
```

value() 메서드를 사용하여 값을 출력하면 10,20이 출력된다. 
```java
    for(Season ses : Season.values()){ 
      System.out.println(ses.value());   // 10 , 20 이 출력된다.
    }
```


### 문자열 초기값을 가지는 Enum


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


### 두 개의 상수 값을 가지는 Enum 

두 개의 속성을 사용하는 Enum 숫자와 문자열 상수 값을 가지는 enum을 정의해 보자. 전체코드는 다음과 같다.

```java

  public enum Lang {

    C(1, "C"), JAVA(2, "Java"), GO(3, "Other");
    
    private int code;
    private String name; 

    public int code() {
      return this.code;
    }
    public String langName() {
      return this.name; 
    }
    private Lang(int code, String name) {
      this.code = code; 
      this.name = name; 
    }
  }
```

추가 상수 값은 다음과 같이 정의한다.

```java
C(1, "C"), JAVA(2, "Java"), GO(3, "Other");
```

첫번재 추가 상수 값과 두번째 추가 상수 값에 대해서 변수를 선언하고 getter를 정의한다.

```java
  private int code;
  private String name; 
  public int code() {
    return this.code;
  }
  public String langName() {
    return this.name; 
  }
```

생성자는 추가 상수 값 순서대로 파라미터를 정의한다.

```java
    private Lang(int code, String name) {
      this.code = code; 
      this.name = name; 
    }
```


## Enum 사용 
### Enum  비교 

Enum은 '==' 연산자를 사용하여 간단히 비교할 수 있다. 
```java
Lang lang = Lang.JAVA;   // Enum 선언
if(lang == Lang.JAVA ){ 
  System.out.println("Lang is java");
}else  { 
  System.out.println("Not equal");
}
```


### switch 구문에서 Enum 사용 
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

## 상수값으로 부터 Enum 객체 생성 
values() 메서드를 이용하여 정의된 enum을 꺼낸 다음에 상수값과 비교하여 인스턴스를 생성해야 한다. 
```java
  int code = 1;
  Lang lang = null; 
  for (Lang myVar : Lang.values()) {
    if(myVar.code() == code) {
      lang = myVar; // instance 생성
    }
  }
  System.out.println(lang);
```

상수 값을 받아 Enum의 인스턴스를 반환하는 코드를 Enum 정의에 static 메서드로 추가할 수 있다. 
```java
  public enum Lang {

    C(1, "C"), JAVA(2, "Java"), GO(3, "Other");
    
    private int code;
    private String name; 

    public int code() {
      return this.code;
    }
    public String langName() {
      return this.name; 
    }
    // Enum 인스턴스를 구한다.
    public static Lang find(int code) {
      for (Lang myVar : Lang.values()) {
        if(myVar.code() == code) {
          return myVar;
        }
      }    
      return null;
    }
    private Lang(int code, String name) {
      this.code = code; 
      this.name = name; 
    }
  }
```

매 번 루프를 돌리는 것 보다 Map에 담아 놓고 쓰는 게 좋다. 
```java

  public enum Lang {

    C(1, "C"), JAVA(2, "Java"), GO(3, "Other");
    
    private int code;
    private String name; 

    public int code() {
      return this.code;
    }
    public String langName() {
      return this.name; 
    }

    private static Map<Integer, Lang> mapToFind = new HashMap<Integer, Lang>();
    static { 
      for (Lang myVar : Lang.values()) {
        mapToFind.put(myVar.code(), myVar);
      }
    }
    public static Lang find(int code) {
      return mapToFind.containsKey(code)? mapToFind.get(code): null;
    }
    private Lang(int code, String name) {
      this.code = code; 
      this.name = name; 
    }
  }
```






