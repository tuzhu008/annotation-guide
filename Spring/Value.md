# @Value

## 定义

```java
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Value {

    /**
     * The actual value expression: for example {@code #{systemProperties.myProp}}.
     */
    String value();

}
```

## 解析

我们可以使用 `@Value` 将属性值注入 bean。它与构造函数、setter和字段注入兼容。

构造函数注入:

```java
Engine(@Value("8") int cylinderCount) {
    this.cylinderCount = cylinderCount;
}
```



