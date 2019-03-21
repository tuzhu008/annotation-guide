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



