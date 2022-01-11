# Interface

Java 8에서는 interface에 대한 몇 가지지가 변경되었다.

* Java 8에서는 default method를 구현할 수 있고 인터페이스를 구현하는 클래스에서 오버라이딩할 수 있다. 
* 인터페이스에 static 메소드를 선언함으로써, 인터페이스를 이용하여 간단한 기능을 가지는 유틸리티성 인터페이스를 만들 수 있게 되었다.

```java
package basic.java8.intf;


import org.junit.jupiter.api.Test;

/** Java 8 Interface Test Case */
public class InterfaceTest {


    /** Interface test 용  */
    private interface MyInterface {
        String getName();
        /** default method */
        default String getAddress(){
            return "Seoul";
        }
        /** static method */
        public static int getAge() {
            return 10;
        }
    }///:~

    /** 구현체 */
    private static class MyImpl implements MyInterface {
        public String getName(){
            return "Honggildong";
        }
    }///~

    @Test
    public void testIntf(){
        MyImpl impl = new MyImpl();
        System.out.println(impl.getName()); // 구현체
        System.out.println(impl.getAddress());  // default method 사용
        // static method는 직접 사용한다.
        System.out.println(MyInterface.getAge());
    }//:

}///~

```
