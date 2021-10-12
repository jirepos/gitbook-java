# Java Date

UTC 시간 값을 DB에 넣고 빼기 위해서 DB와 SYSTEM의 TimeZone을 UTC로 설정하면 좋겠지만, 현실적으로 서버 타임존을 변경할 수 없는 경우가 많기 때문에 프로그램적으로 처리하는 방법을 고민해야 한다.

DB에 값을 UTC로 넣는다고 하더라도 쿼리 툴에서 작업할 때 SYSTDATE 같은 함수를 사용하면 혼동이 생길 수 있기 때문에 적정하지 않는 것 같다.

값은 서버 시스템의 타임존 값으로 DB에 넣고 보여줄 때 UTC로 보여주거나, 사용자의 타임존으로 변경해서 보여 주어야 할 것 같다.

JSON에서 시간 형식

"2020-05-11T02:00:00.000Z"

*   T는 날짜와 시간을 구분해주는 delimiter(구분자)이다. 그이외의 의미는 없다.

    끝에 Z는 영국 그리니치 천문대 시간을 가르킨다(+00:00). 한국은 (+09:00)이다.
* Z = Zulu time = GMT = UTC 다 같은말이다.

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
