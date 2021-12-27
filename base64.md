# BASE64 

BASE64 encoding/decoding을 하려면 Java 8에 추가된 Base64.getEncoder()와 Base64.getDecoder()를 사용한다. 


## 기본 사용 
```java
  /** 기본 encode/decode */
  @Test
  public void testEncodeDecode() {
    String str = "이것은 원본 original 문자열입니다.ㅋㅋa"; 
    byte[] encodedByte = Base64.getEncoder().encode(str.getBytes()); // byte로 변환 
    String encodedStr = new String(encodedByte); // 다시 문자열로 
    System.out.println(encodedStr); // 문자여 출력 
  }//:
```
결과는 다음과 같다. 
```shell
7J206rKD7J2AIOybkOuzuCBvcmlnaW5hbCDrrLjsnpDsl7TsnoXri4jri6Qu44WL44WLYQ==
```

다음과 같이 decode하여 원래의 문자열을 출력한다. 
```java
  /** 기본 decode */
  @Test
  public void testDecodeBasic() {
    String str = "이것은 원본 original 문자열입니다.ㅋㅋa"; 
    byte[] encodedByte = Base64.getEncoder().encode(str.getBytes()); // byte로 변환 
    byte[] decodedByte = Base64.getDecoder().decode(encodedByte);
    String decodedStr = new String(decodedByte);
    System.out.println(decodedStr);
  }
```
결과는 다음과 같다. 
```shell
이것은 원본 original 문자열입니다.ㅋㅋa
```

## 문자열로 encode/decode
encodeToString()을 사용하면 문자열로 변환할 수 있다. 
```java
  /** 문자열로 encode */
  @Test 
  public void testEncodeString() {
    String str = "이것은 원본 original 문자열입니다.ㅋㅋa"; 
    String encodedStr = Base64.getEncoder().encodeToString(str.getBytes());
    System.out.println(encodedStr);
  }
```
다음과 같이 decode 한다. 
```java
  /** 문자열로 dncode */
  @Test 
  public void testDecodeString() {
    String encodedStr = "7J206rKD7J2AIOybkOuzuCBvcmlnaW5hbCDrrLjsnpDsl7TsnoXri4jri6Qu44WL44WLYQ==";
    byte[] decodedBytes = Base64.getDecoder().decode(encodedStr);
    System.out.println(new String(decodedBytes));
  }
```

## 패딩 없는 encode 

Base64 인코딩에서 출력 인코딩 된 문자열의 길이는 3의 배수여야 한다.  
그렇지 않은 경우 출력은 추가 패드 문자 ( = ) 로 채워진다. 

어떤 이유에서인지 패딩이 필요 없으면 다음과

```java
  /** 패딩없는 encode */
  @Test 
  public void testEncodeStringWithoutPadding() {
    String str = "이것은 원본 original 문자열입니다.ㅋㅋa"; 
    String encodedStr = Base64.getEncoder().withoutPadding().encodeToString(str.getBytes());
    System.out.println(encodedStr);
  }
```
결과는 다음과 같다. 
```shell
7J206rKD7J2AIOybkOuzuCBvcmlnaW5hbCDrrLjsnpDsl7TsnoXri4jri6Qu44WL44WLYQ
```
> 주의: 위에서 encoded된 문자열은 decode 안된다. 


## MIME encoding 
MIME 인코더는 기본 알파벳을 사용하지만 MIME 친화적 인 형식으로 Base64로 인코딩 된 출력을 만들어야 하고 출력의 각 행은 76 자 이하이며 캐리지 리턴과 라인 피드 ( \ r \ n )로 끝나야 한다. 
```java

  /** Mime Encode */
  @Test
  public void testMimeEncode() {
    // 출력의 각 행의 76자를 넘어서는 안된다. 
    // 캐리지 리턴과 라인피드(\r\n)로 끝난다. 
    String str = "Base64 인코딩에서 출력 인코딩 된 문자열의 길이는 3의 배수여야 한다."
       + "그렇지 않은 경우 출력은 추가 패드 문자 ( = ) 로 채워진다."
       + "5년 만의 미국 출장을 마친 이재용 삼성전자 부회장이"
       + "우리 현장의 목소리와 시장의 냉혹한 현실을 직접 보고 오게 되니 마음이 무겁다고 밝혔다";
       String encodedStr = Base64.getMimeEncoder().encodeToString(str.getBytes());
       System.out.println(encodedStr);
  }//:
```
결과는 다음과 같다. 

```shell
QmFzZTY0IOyduOy9lOuUqeyXkOyEnCDstpzroKUg7J247L2U65SpIOuQnCDrrLjsnpDsl7TsnZgg
6ri47J2064qUIDPsnZgg67Cw7IiY7Jes7JW8IO2VnOuLpC7qt7jroIfsp4Ag7JWK7J2AIOqyveya
sCDstpzroKXsnYAg7LaU6rCAIO2MqOuTnCDrrLjsnpAgKCA9ICkg66GcIOyxhOybjOynhOuLpC41
64WEIOunjOydmCDrr7jqta0g7Lac7J6l7J2EIOuniOy5nCDsnbTsnqzsmqkg7IK87ISx7KCE7J6Q
IOu2gO2ajOyepeydtOyasOumrCDtmITsnqXsnZgg66qp7IaM66as7JmAIOyLnOyepeydmCDrg4nt
mLntlZwg7ZiE7Iuk7J2EIOyngeygkSDrs7Tqs6Ag7Jik6rKMIOuQmOuLiCDrp4jsnYzsnbQg66y0
6rKB64uk6rOgIOuwne2YlOuLpA==
```
MIME 타입을 decode하려면 getMimeDecoder()를 사용한다. 
```java
       // decode 
       byte[] decodedBytes = Base64.getMimeDecoder().decode(encodedStr);
       System.out.println(new String(decodedBytes));
```


## URL Encode/Decode 
URL 및 파일 이름은 Safe Base 64를 사용하여 줄 구분을 추가하지 않는다.  getUrlEncoder()를 사용하여 encode한다. getUrlDecoder()를 사용하여 decode한다. 

```java
  @Test 
  public void testUrlEncode() {
    String url = "https://www.demo.com?abc_dd=absF&gw_ql#q=aa1";
    String encodedUrl = Base64.getUrlEncoder().encodeToString(url.getBytes());
    System.out.println(encodedUrl);
    byte[] decodedBytes = Base64.getUrlDecoder().decode(encodedUrl);
    String decodedUrl = new String(decodedBytes);
    System.out.println(decodedUrl);
  }
```
> URL에 Safe한 Base 64:
> BASE64에서는 62, 63을 보면 각각 +, / 에 대응되는 것을 알 수 있다. 그러나 이는 데이터를 컨트롤할 때 오류를 일으킬 수 있는 문제가 될 수도 있고 = 가 url에서 사용하는 문자라는 문제 등이 제기 되었다. 따라서 url safe 한 base64 에서는 62, 63번은 - , _로 대체되었다. 




## References

https://recordsoflife.tistory.com/331


