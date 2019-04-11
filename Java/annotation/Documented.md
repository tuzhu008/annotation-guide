# @Documented

## 定义

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Documented {
}
```

## 解析

`@Documented`  注解指出，使用该注解的元素应该由 JavaDoc 进行文档化。例如:

```java
java.lang.annotation.Documented
@Documented
public @interface MyCustomAnnotation {
  //Annotation body
}
```

```java
@MyCustomAnnotation
public class MyClass { 
     //Class body
}
```

在为类 MyClass 生成 javadoc 时，将包含 `@MyCustomAnnotation`。

