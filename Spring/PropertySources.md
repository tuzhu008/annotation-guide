# @PropertySources

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface PropertySources {

    PropertySource[] value();

}
```

## 解析

我们可以使用这个注解来指定多个 `@PropertySource` 配置:

