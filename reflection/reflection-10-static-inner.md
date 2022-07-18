# Java Reflection - Static Inner class

## 클래스 인스턴스 생성 
Static inner class를 가진 class를 정의한다. 
```java
package basic.java8.reflection;

/** Reflection Test를 위한 Class */
public class TestInner {
    private int age;
    public int getAge() {
        return this.age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public TestInner(int age) {
        this.age = age;
    }
    public static class TypeHandler {
        String name;
        public String getName(){
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        public TypeHandler(String name) {
            this.name = name;
        }
    }// inner class

}

```
다음과 같이 클래스의 인스턴스 및 static inner class의 인스턴스를 생성할 수 있다. 

```java
package basic.java8.reflection;

import org.junit.jupiter.api.Test;

import java.lang.reflect.Constructor;

/** Reflection Test */
public class ReflectionTest {
    /** 생성자를 사용한 인스턴스 생성 */
    @Test
    public void testCreateInnerClass() throws Exception {
        // inner class를 감싸고 있는 클래스의 생성
        Class<?> clazz = Class.forName("basic.java8.reflection.TestInner");
        Constructor<?> constructor = clazz.getConstructor(int.class); // 생성자 타입 설정, 없으면 null
        TestInner ti  = (TestInner)constructor.newInstance(10); // 생성자에 파라미터 전달
        System.out.println(ti.getAge());

        // Static innner class 생성
        Class<?> innerClass = Class.forName("basic.java8.reflection.TestInner$TypeHandler");
        Constructor<?> innerConstructor = innerClass.getConstructor(String.class); // 생성자 타입 전달, 없으면 null
        // 생성자 파라미터 전달
        TestInner.TypeHandler typeHandler = (TestInner.TypeHandler) innerConstructor.newInstance("홍길동");
        System.out.println(typeHandler.getName());
    }//:

}///~
```


