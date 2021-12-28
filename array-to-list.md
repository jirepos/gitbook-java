# Array와 List간의 변환 

## Array를 List로 
```java
    /**  Array를 리스트로 */
    @Test
    public void testArraytoList(){
        List<String> list = Arrays.asList("가", "나", "다");
        for (String s : list) {
            System.out.println(s);
        }
    }//:
```


## List를 Array로 
```java
   /** List를 Array 변환 */
    @Test
    public void testListToArray(){
        List<String> list = Arrays.asList("가", "나", "다");
        //Object[] objs = list.toArray();
        String[] arrs = list.toArray(new String[list.size()]);
        for (String s : arrs) {
            System.out.println(s);
        }
    }//:
```