# Charset 

문자열에 특정 chartset을 정의하려면 Charset 클래스를 사용한다. 

> 기본적으로 UTF-8을 사용한다. 


```java
java.nio.charset.Charset
```
Charset을 구하려면 StandardCharsets 클래스를 사용한다. 
```java
byte[] bytes = rawString.getBytes(StandardCharsets.UTF_8);
```
UTF-8을 사용하여 byte[]을 구하고 다시 문자열로 encodding 하는 테스트 케이스는 아래와 같다. 

```java

  @Test 
  public void testCharset() {
    String rawString = "Entwickeln Sie mit Vergnügen";
    byte[] bytes = rawString.getBytes(StandardCharsets.UTF_8);
    String utf8EncodedString = new String(bytes, StandardCharsets.UTF_8);
    System.out.println(utf8EncodedString);
  }//~
```
