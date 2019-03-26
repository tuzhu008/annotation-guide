# @Target

## 定义

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Target {
    /**
     * Returns an array of the kinds of elements an annotation type
     * can be applied to.
     * @return an array of the kinds of elements an annotation type
     * can be applied to
     */
    ElementType[] value();
}
```

## 解析

它指定了我们可以在哪里使用注解。例如: 在下面的代码中，我们将目标类型定义为 `METHOD`，这意味着下面的注释只能用于方法。

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target({ElementType.METHOD})
public @interface MyCustomAnnotation {

}
```

```
public class MyClass {
   @MyCustomAnnotation
   public void myMethod()
   {
       //Doing something
   }
}

```



