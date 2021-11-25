# Java 8 Date

표준 날짜/시간에  대한 처리를 위한 기준을 정한다. 
* DB에는 UTC 값을 저장한다. 
* DB에서는 SYSDATE와 같은 명령을 사용하지 않는다.
* **날짜/시간의 표준 문자열 형식은 ISO 8601을 사용한다.** 
* 서버와 클라이언트는 ISO 8601 형식으로 주고 받는다. 



**ISO 8601 형식** 
자세한 것은 [위키](https://ko.wikipedia.org/wiki/ISO_8601)를 참고한다. 

```
2020-05-11T02:00:00.000Z
2017-03-16T17:40:00+09:00
```

* T는 날짜와 시간을 구분해주는 delimiter(구분자)이다. 그이외의 의미는 없다.
* 끝에 Z는 영국 그리니치 천문대 시간을 가르킨다(+00:00). 한국은 (+09:00)이다.
* Z = Zulu time = GMT = UTC 다 같은말이다.



## Java 8의 시간 관련 클래스 

### Local 
로컬 시간을 표시하는 클래스들을 알아 보자.  Local이 붙은 것들은 TimeZone에 대한 정보를 가지지 않는다. 따라서 TimeZone에 따른 시간 변환이 불가능하다. 

* LocalDate 날짜 정보. 시간 없음. 
* LocalTime 시간 정보. 날짜 없음. 
* LocalDateTime  날짜,시간 정보 있음(LocalDate + LocalTime)

**현재 날짜 구하기** 

```java
  /** 현재 날짜 구하기  */
  @Test 
  void testLocalDateNow() {
    LocalDate today = LocalDate.now(); 
    // 시간 값 정보 없음 
    System.out.println(today);  // 2021-11-25 
    LocalDate myDate = LocalDate.of(2001, 9, 3 );
    System.out.println(myDate);  //2001-09-03
    LocalDate christMas = LocalDate.parse("2021-12-25");
    System.out.println(christMas);  // 2021-12-25
  }//:
```

**현재 시간 구하기** 

```java
  @Test 
  void testLocalTimeNow() {
    LocalTime currentTime = LocalTime.now();
    System.out.println(currentTime);  // 15:10:55.358240200
    LocalTime curParis = LocalTime.now(ZoneId.of("Europe/Paris"));
    System.out.println(curParis);  //07:11:26.926995500
    LocalTime aTime = LocalTime.of(21, 20, 0);
    System.out.println(aTime); // 21:20
    LocalTime plusTime = aTime.plusHours(8);
    System.out.println(plusTime);  //05:20
  }
```
LocalTime에는 plusMinutes() 메소드도 제공한다. 

**현재 날짜/시간 구하기** 

```java
  /** LocalDateTime Test */
  @Test
  void testLocalDateTime() {
    LocalDateTime now = LocalDateTime.now();
    System.out.println(now);  //2021-11-25T15:26:51.886868600
    LocalDateTime now2 = LocalDateTime.of(LocalDate.now(), LocalTime.now());  
    System.out.println(now2);//2021-11-25T15:26:51.886868600

    LocalDateTime dt1 = LocalDateTime.parse("2020-12-31T23:59:59.999");
    System.out.println(dt1); //2020-12-31T23:59:59.999
    LocalDateTime dt2 = LocalDateTime.of(2022, 5, 6, 15, 30, 00);
    System.out.println(dt2);  //2022-05-06T15:30
    LocalDateTime dt3 = Year.of(2022).atMonth(5).atDay(6).atTime(15, 30);
    System.out.println(dt3);  //2022-05-06T15:30
  }
```

**Utility Methods** 
**LocalDate** 
```java
 @Test
  void testLocalDateUtility() {
    LocalDate now = Year.of(2021).atMonth(11).atDay(24);
    System.out.println(now);  // 2021-11-24
    System.out.println(now.getYear());  // 2021
    System.out.println(now.getMonth()); // NOVEMBER
    System.out.println(now.getDayOfMonth()); // 24
    System.out.println(now.getDayOfWeek()); // WEDNESDAY
    System.out.println(now.isLeapYear());  // false , 윤년아님 
    System.out.println(now.plusYears(1));  // 2022-11-24
    System.out.println(now.plusMonths(1)); // 2021-12-24
    System.out.println(now.plusDays(1)); // 2021-11-25

    LocalDate yesterday = now.minusDays(1);
    System.out.println(now.isAfter(yesterday));  // true 
    System.out.println(now.isBefore(yesterday)); // false 
  }
```
**LocalDateTime**
```java
  @Test 
  void testLocalDateTimeUtility() {
    LocalDate date = Year.of(2021).atMonth(11).atDay(24);
    LocalDateTime now = date.atTime(21, 50, 10, 888);
    System.out.println(now);  //2021-11-24T21:50:10.000000888
    System.out.println(now.getHour()); // 21
    System.out.println(now.getMinute()); // 50
    System.out.println(now.getSecond()); // 10 
    System.out.println(now.getNano()); // 888
  }
```


### Instant

자바8 Time API의 Instant 클래스는 시간을 타임스탬프로 다루기 위해서 사용한다. 타임스탬프는 UTC 기준으로 1970년 1월 1일 0시 0분 0초를 숫자 0으로 정하고 그로 부터 경과된 시간을 양수 또는 음수로 표현한다. 

```java
  @Test
  void testInstant() {
    Instant epoch = Instant.EPOCH; // Instant.ofEpochSecond(0); 와 동일
    System.out.println(epoch);  //1970-01-01T00:00:00Z
    Instant futureEpoch = Instant.ofEpochSecond(1000_000_000);  
    System.out.println(futureEpoch); // 2001-09-09T01:46:40Z
    Instant pastEpoch = Instant.ofEpochSecond(-1_000_000_000);
    System.out.println(pastEpoch);  //1938-04-24T22:13:20Z
  }
```

**현재 시간 구하기**
```java

  @Test
  void testInstantNow() {
    Instant now = Instant.now();
    System.out.println(now);  // 2021-11-25T06:59:29.930470500Z
    long epochSecond = now.getEpochSecond();  // 초 단위 타임스탬프
    System.out.println(epochSecond);  // 1637823569
    long epochMilli = now.toEpochMilli();  // 밀리세컨드 단위 타임 스탬프 
    System.out.println(epochMilli); //1637823569930
  }
```

### ZonedDateTime

ZonedDateTime 클래스는 타임존 또는 시차 개념이 필요한 날짜와 시간 정보를 나타내기 위해서 사용한다. 

```java
  @Test 
  void testZonedDateTime() {
    ZonedDateTime nowLocal = ZonedDateTime.now();
    System.out.println(nowLocal);  //2021-11-25T16:04:19.396320300+09:00[Asia/Seoul]

    ZonedDateTime curSeoul = ZonedDateTime.now(ZoneId.of("Asia/Seoul"));
    System.out.println(curSeoul); //2021-11-25T16:04:53.223393100+09:00[Asia/Seoul]
    
    ZonedDateTime specDt = ZonedDateTime.of(2010, 7, 5, 10, 10, 10, 25, ZoneId.of("Asia/Seoul"));
    System.out.println(specDt);    // 2010-07-05T10:10:10.000000025+09:00[Asia/Seoul]
  }
```


## TimeZone 이해 
ZonedDateTime은 LocalDateTime에 타임존 또는 시차 개념이 추가되어 있다고 생각하면 쉽게 이해할 수 있다. 

```shell
|--------------------ZonedDateTime--------------------|			
|--------------OffsetDateTime--------------|			
|-------LocalDateTime-------|			
|--LocalDate--|--LocalTime--|--ZoneOffset--|--ZoneId--|
   2021-01-05	   12:00:00	       +09:00	    Asia/Seoul
```
### LocalDateTime에 타임존 설정
```java
  /** LocalDateTime에 타임존 정보 설정 */
  @Test 
  void testTimeDiff() {
    LocalDate ld = LocalDate.of(2021, 11, 25);
    LocalTime lt = LocalTime.of(16, 30);
    LocalDateTime ldt = LocalDateTime.of(ld, lt);
    // LocalDateTime에 타임존 설정 
    ZonedDateTime zonedDateTime = ZonedDateTime.of(ldt, ZoneId.of("Asia/Seoul"));
    System.out.println(zonedDateTime); // 2021-11-25T16:30+09:00[Asia/Seoul]

    System.out.println(zonedDateTime.toLocalDateTime());  //2021-11-25T16:30
    System.out.println(zonedDateTime.toLocalDate()); // 2021-11-25
    System.out.println(zonedDateTime.toLocalTime()); // 16:30
  }
```


### ZoneID and ZoneOffset 

ZoneId은 타임존, ZoneOffset은 시차를 나타낸다. ZoneOffset는 UTC 기준으로 고정된 시간 차이를 양수나 음수로 나타내는 반면에 ZoneId는 이 시간 차이를 타임존 코드로 나타낸다. 서울의 경우 타임존 코드는 Asia/Seoul이고 시차는 +0900 이다. 

```java
  @Test 
  void testZoneOffset() {
    ZoneOffset seoulZoneOffset = ZoneOffset.of("+09:00"); // Seoul
    System.out.println(ZonedDateTime.now(seoulZoneOffset));  //2021-11-25T16:23:59.080782400+09:00

    ZoneOffset laZonedOffset = ZoneOffset.of("-08:00"); // LA
    System.out.println(ZonedDateTime.now(laZonedOffset));  // 2021-11-24T23:26:35.777549900-08:00
  }
```

그러나 일광 정력타임을 사용하는 도시들이 있기 때문에 시차보다는 ZoneId 를 사용해야 한다. 

```java
  @Test 
  void testZoneIdTset() {
    ZoneId vancouverZoneId = ZoneId.of("America/Vancouver"); // 여기는 DST사용 
    System.out.println(ZonedDateTime.now(vancouverZoneId));  //2021-11-24T23:29:54.191862300-08:00[America/Vancouver]
  }
```

### ZoneId 출력 
ZoneId.getAvailableZoneIds()를 사용하여 사용 가능한  ZoneId를 구할 수 있다. 
```java
  /** ZoneId 출력 */
  @Test
  void testListZoneIds() {
   LocalDateTime localDateTime = LocalDateTime.now();
    for (String zoneId : ZoneId.getAvailableZoneIds()) {
      ZoneId id = ZoneId.of(zoneId);
      // LocalDateTime -> ZonedDateTime
      ZonedDateTime zonedDateTime = localDateTime.atZone(id);
      // ZonedDateTime -> ZoneOffset
      ZoneOffset zoneOffset = zonedDateTime.getOffset();
      //replace Z to +00:00
      String offset = zoneOffset.getId().replaceAll("Z", "+00:00");
      System.out.format("%s (%s)\n", id.getId(), offset);
    }
  }
```
결과는 다음과 같다. 
```shell
Asia/Aden (+03:00)
America/Cuiaba (-03:00)
Etc/GMT+9 (-09:00)
Etc/GMT+8 (-08:00)
Africa/Nairobi (+03:00)
... 생략 ....
```      

### 다른 타임존 시차 적용 
Zoneoffset 또는 ZoneId를 사용한 시차 적용을 할 수 있다. 
```java
  /** 다른 타임존/시차 적용 */
  @Test 
  void testSetTimeZone() {
    // 서울 타임존의 시간 설정
    ZonedDateTime seoul = Year.of(2021).atMonth(11).atDay(25).atTime(16, 43).atZone(ZoneId.of("Asia/Seoul"));
    // UTC로 변경
    ZonedDateTime utc = seoul.withZoneSameInstant(ZoneOffset.UTC);
    // 9시간 차이가 나니까, 07시가 된다. 
    System.out.println(utc);  // 2021-11-25T07:43Z
    // 14시간 차이 
    ZonedDateTime toronto = seoul.withZoneSameInstant(ZoneOffset.of("-0500"));
    System.out.println(toronto);  //2021-11-25T02:43-05:00
  }//:
```  

## 문자열 변환 
### ZonedDateTIme을 문자열로 변환 
ZonedDateTime의 format() 메소드에 DateTimeFormatter 객체를 전달하여 다양한 포멧의 문자열로 변환할 수 있다. 
자세한 사용법은 아래 링크를 참조한다. 
[DateTimeFormatter](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/format/DateTimeFormatter.html)


```java
  /** 문자열 변환 Test  */
  @Test 
  void testTimeZoneToString() {
    // Format 형식은 아래 URL 참조
    // https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/format/DateTimeFormatter.html
    ZonedDateTime seoul = Year.of(2021).atMonth(11).atDay(25).atTime(16, 43).atZone(ZoneId.of("Asia/Seoul"));
   
    System.out.println(seoul.format(DateTimeFormatter.ISO_OFFSET_DATE_TIME));  // 2021-11-25T16:43:00+09:00
    System.out.println(seoul.format(DateTimeFormatter.ISO_DATE_TIME));  // 2021-11-25T16:43:00+09:00[Asia/Seoul]
    System.out.println(seoul.format(
          DateTimeFormatter.ofPattern("yyyy/MM/dd/ HH:mm:ss z")));  //2021/11/25/ 16:43:00 KST
    System.out.println(seoul.format(
          DateTimeFormatter.ofLocalizedDateTime(FormatStyle.FULL)));  //2021년 11월 25일 목요일 오후 4시 43분 0초 대한민국 표준시
    System.out.println(seoul.format(
          DateTimeFormatter.ofLocalizedDateTime(FormatStyle.FULL).withLocale(Locale.KOREA)));   //2021년 11월 25일 목요일 오후 4시 43분 0초 대한민국 표준시
    // 2020-05-11T02:00:00.000Z
    //DateTimeFormatter.iso

    ZonedDateTime toronto = seoul.withZoneSameInstant(ZoneOffset.of("-0500"));
    // ZoneId가 없으면 Zone을 표시하지 못한다. 
    System.out.println(toronto.format(DateTimeFormatter.ISO_DATE_TIME));  //2021-11-25T02:43:00-05:00
    System.out.println(toronto.format(DateTimeFormatter.ISO_OFFSET_DATE_TIME));  // 2021-11-25T16:43:00+09:00
    ZonedDateTime toronto2 = seoul.withZoneSameInstant(ZoneId.of("America/Toronto"));
    System.out.println(toronto2.format(DateTimeFormatter.ISO_DATE_TIME));  //2021-11-25T02:43:00-05:00[America/Toronto]
    System.out.println(toronto2.format(DateTimeFormatter.ISO_OFFSET_DATE_TIME));  // 2021-11-25T02:43:00-05:00
  }//:
```

> 가장 정확하게는 ZoneId까지 주고 받아야 하지만 사용자가 선택한 ZoneId를 저장한 경우에는 저장한 값을 활용하여 계산할 수도 있다. 


### 문자열을 ZonedDateTime으로 변환 
```java
  /** 문자열을 ZonedDateTime으로 변환  */
  @Test 
  void testStringToZoneDateTime() {
    ZonedDateTime zdt1 = ZonedDateTime.parse("2021-11-25T16:43:00+09:00[Asia/Seoul]");
    ZonedDateTime zdt2 = ZonedDateTime.parse("2021/11/25/ 16:43:00 KST", DateTimeFormatter.ofPattern("yyyy/MM/dd/ HH:mm:ss z"));
    System.out.println(zdt1);
    System.out.println(zdt2);
  }//:
```




## 응용하기 
### 현재 시간 구하기






## 아래는 정리할 내용 

Calendar.getInstance() 시스템의 현재 날짜와 시간 정보를 얻기 위해 사용한다. java.util.Date를 반환한다.

```java
public void testDate() {
  Date date =  Calendar.getInstance().getTime();
  System.out.println(date.toString());
}
```

```
  Thu Jul 29 11:59:50 KST 2021
```

Calendar 클래스로 UTC를 다루기에는 부적합하다.

현재 날짜시간값을 UTC값으로 구하고 싶다면 Instant.now()를 사용한다.1970년 1월 1일 UTC의 첫 번째 순간 이후의 현재 시간까지의 나노초를 나타낸 값 입니다.

```java
public void testInstant() {
  Instant today = Instant.now();
  System.out.println(today.toString());
  // Converts this instant to the number of milliseconds 
  // from the epoch of
  // 1970-01-01T00:00:00Z.
    System.out.println(today.toEpochMilli());
}// :
```

```
2021-07-29T03:19:30.234220900Z
1627528770234
```

> java.sql.Date는 java.util.Date를 상속 받는다. 그런데 시/분/초에 대한 값이 없다.

그럼, DB에 값을 넣기 위해서는 Java Bean의 필드를 java.util.Date 형식으로 설정해야 한다.

```java
public class Person {
  private java.util.Date regDate; 
}
```

그러면, DB에는 System의 TimeZone의 값이 입력이된다. DB에 UTC를 넣을 수는 없는가? Java Bean에서 java.util.Date를 써야 한다면 Instant를 Date로 변환해야 한다. Date.from(Instatn)를 사용하면 된다.

```java
Instant today = Instant.now();
// Date로 변환하면 시스템의 값으로 변경됨 
Date d = Date.from(today);
```

그런데 문제는 Date 인스턴스의 값을 출력해보면 UTC가 아니라 시스템 타임존의 시간값이 출력된다.

해결방법

Date 시스템의 현재 시간 값을 구하고 Date - offset 그 값에서 UTC와의 시차만큼 값을 뺀다. 그럼 Date값은 UTC가 된다.

그러면 그냥 시스템의 시간값을 그냥 입력하고 보여줄 때 변환한다.

mysql에서 날짜와 시간형식

* date: 날짜 (YYYY-MM-DD)
* datetime: 날짜와 시간(YYYY-MM-DD hh:mm:ss)
* timestamp: 날짜와 시간(YYYY-MM-DD hh:mm:ss\[.fraction])

datetime과 timestamp 둘다 날짜와 시간을 적을 수 있는데 가장큰 차이는 타임 존 반영 유무다.

Oracle에서 날짜와 시간 형식 create table time_test (a date, b timestamp(9), c timestamp with time zone, d timestamp with local time zone);


```shell
// 로컬 시간을 의미하는 ISO 8601 문자열
2021-10-06T15:00:00.000
// UTC(GMT) 시간을 의미하는 ISO 8601 문자열
2021-10-06T06:00:00.000Z

// 로컬 시간을 의미하면서 UTC(GMT) 대비 +09:00 임을 의미하는 ISO 8601 문자열
2021-10-06T15:00:00.000+09:00
```
- `2021-10-06T15:00:00.000`은 **ISO 8601**의 기본 형식이다. 해당 시간이 로컬 시간 임을 의미한다.
- `2021-10-06T06:00:00.000Z`와 같이 뒤에 `Z` 식별자를 추가하면 해당 시간이 **UTC(GMT)** 시간 임을 의미한다.
- `2021-10-06T15:00:00.000+09:00`와 같이 뒤에 **Z** 대신 `+HH:mm` 식별자를 추가하면 해당 시간이 로컬 시간이면서 **UTC(GMT)**와 **09:00** 만큼 차이가 남을 의미한다. 이 형식의 장점은 인간이 손쉽게 추가적인 계산 없이 로컬 시간을 인지하면서 추가적으로 타임존 정보까지 제공하기 때문에 가장 인간친화적이라고 할 수 있다.



## References 
[Local 시간 사용](https://www.daleseo.com/java8-local-date-time/) 

https://www.daleseo.com/java8-zoned-date-time/
https://www.daleseo.com/java8-instant/
