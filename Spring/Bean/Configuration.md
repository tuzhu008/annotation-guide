# @_Configuration_

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {

    /**
     * Explicitly specify the name of the Spring bean definition associated with the
     * {@code @Configuration} class. If left unspecified (the common case), a bean
     * name will be automatically generated.
     * <p>The custom name applies only if the {@code @Configuration} class is picked
     * up via component scanning or supplied directly to an
     * {@link AnnotationConfigApplicationContext}. If the {@code @Configuration} class
     * is registered as a traditional XML bean definition, the name/id of the bean
     * element will take precedence.
     * @return the explicit component name, if any (or empty String otherwise)
     * @see org.springframework.beans.factory.support.DefaultBeanNameGenerator
     */
    @AliasFor(annotation = Component.class)
    String value() default "";

}
```

## 解析

`@Configuration` 注解的类说明这个类的主要是作为一个 bean 定义的资源文件。

_Configuration _类可以包含用 @Bean 注解的 bean 工厂方法:

```java
@Configuration
class VehicleFactoryConfig {

    @Bean
    Engine engine() {
        return new Engine();
    }

}
```

上面的 VehicleFactoryConfig 类和 Spring XML 的配置是等价的：

```xml
<beans>
    <bean id="engine" class="com.aho.services.Engine"/>
</beans>
```



