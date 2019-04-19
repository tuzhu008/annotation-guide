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

```java
@Configuration
@PropertySources({ 
    @PropertySource("classpath:/annotations.properties"),
    @PropertySource("classpath:/vehicle-factory.properties")
})
class VehicleFactoryConfig {}
```

注意，从 Java 8 开始，我们可以通过上面描述的重复注解特性实现相同的功能。

