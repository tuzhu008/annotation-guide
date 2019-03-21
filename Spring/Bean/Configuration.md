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

_Configuration _类可以包含用 @Bean 注解的 bean 工厂方法:

