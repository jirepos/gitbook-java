#  Java Reflection - Constructor 
```java
import java.lang.reflect.Constructor;
```

```java

     /** Reflection Test를 위한 inner class */
     @Deprecated
     static class MyClass {
         public String name; 
         public MyClass() {
         }
         public MyClass(String name) {
             this.name = name;
         }
         public String getName(){ 
             return name; 
         }
         public void setName(String name) {
             this.name = name; 
         }
     }
```

```java
   
    /** 생성자를 구하고 생성자의 파라미터 타입을 확인한다. */
    @Test
    public void testConstructor() throws Exception {
        Class<?> aClass = MyClass.class; //obtain Class object. See prev. section
        Constructor<?> constructor = aClass.getConstructor(new Class[]{String.class});
        System.out.println(constructor.getName()); //basic.java8.reflection.RefTestClass$MyClass
        Class<?>[] parameterTypes = constructor.getParameterTypes();
        for (Class<?> parameterType : parameterTypes) {
            System.out.println(parameterType.getName());  //java.lang.String
        }
    }
```         