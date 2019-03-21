# @Lookup

## 定义

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Lookup {

    /**
     * This annotation attribute may suggest a target bean name to look up.
     * If not specified, the target bean will be resolved based on the
     * annotated method's return type declaration.
     */
    String value() default "";

}
```

## 解析

使用 `@Lookup` 注解的方法告诉 Spring 在调用方法时返回方法返回类型的实例。



有关注释的详细信息可以在本文中找到。

