# 익명 클래스 

## 추상클래스를 사용한 익명 클래스 
```java
public abstract class AnnoAbstract {
  public abstract int getAge();
}
```
다음과 같이 구현하여 사용한다. 
```java
   @Test
   public void testAnno1(){ 
     AnnoAbstract ab = new AnnoAbstract() {
       @Override 
       public int getAge() {
         return 10; 
       }
     };
     System.out.println(ab.getAge());
   }
```


## 인터페이스를 이용한 익명 클래스 

```java
  public  interface UserBaseInfo { 
    String getName(); 
  }
```
다음과 같이 구현하여 사용한다. 
```java
   /** 인터페이스를 이용한 익명 클래스 선언 */
   @Test 
   public void testAnno2(){ 
     UserBaseInfo ub = new UserBaseInfo() { 
       @Override 
       public String getName(){ 
         return "Hong";
       }
     };

     System.out.println(ub.getName());

   }
```


## 코드 블럭 
코드 블럭은 클래스가 생성될 때 실행된다.  코드 블럭에서는 ONE(), TWO() 메서드를 호출한다. 
```java
  public static class InnoTwo { 
    private List<String> list = new ArrayList<String>();

    /** code 블럭  */
    {
      // 코드 블럭은 InnoTwo 클래스가 생성될 때 호출 된다. 
       ONE("FIRST");
       TWO("SECOND");
    }
    private List<String> getList(){ 
      return this.list; 
    }
    private void ONE(String str){ 
      list.add(str);
    }
    private void TWO(String str){ 
      System.out.println(str);
    }
   }
```
다음과 같이 InnoTwo 클래스를 생성하면 코드블럭이 실행된다. 
```java
   @Test 
   public void testAnno3(){ 
     InnoTwo two = new InnoTwo();
     two.getList().forEach( item -> { System.out.println(item); });
   }
```


### MyBatis SQL() 코드 이해 
다음과 같은 코드는 익명 클래스를 생성하면서 코드 블럭을 정의한다는 것을 알 수 있다. 그러나 자세한 내부 구조는 아직 모른다. 
```java
  public static String getBlogPost() {
    return new SQL() {
      /** 코드 블럭은 익명 클래스가 생성될 때 호출 된다. */
      {
        SELECT("*");
        FROM("blog_post");
      }
    }.toString();
  }//:
```




