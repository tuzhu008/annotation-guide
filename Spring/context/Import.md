# @Import

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Import {

    /**
     * {@link Configuration}, {@link ImportSelector}, {@link ImportBeanDefinitionRegistrar}
     * or regular component classes to import.
     */
    Class<?>[] value();

}
```

## 解析

使用这个注解，我们可以使用特定的 `@Configuration` 类，而不需要扫描组件。我们可以用 `@Import` 的 `value` 参数来提供这些类:

```java
@Import(VehiclePartSupplier.class)
class VehicleFactoryConfig {}
```



