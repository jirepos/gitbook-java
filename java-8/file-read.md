# 파일 읽기 


```java
String fileName = "D:/file.txt";
List<String> lines = Files.readAllLines(Paths.get(fileName), Charset.forName("euc_kr"));
lines.forEach(System.out::println);
```
