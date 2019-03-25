# @Documented

## 定义

```

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



