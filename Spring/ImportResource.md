# @ImportResource

## 定义

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
public @interface ImportResource {

    /**
     * Alias for {@link #locations}.
     * @see #locations
     * @see #reader
     */
    @AliasFor("locations")
    String[] value() default {};

    /**
     * 要从其中导入的资源位置。
     * 支持资源加载前缀，如 {@code classpath:},}, {@code file:}, 等.
     * 有关如何处理资源的详细信息，请参考 Javadoc 中的 {@link #reader}。
     * @since 4.2
     * @see #value
     * @see #reader
     */
    @AliasFor("value")
    String[] locations() default {};

    /**
     * {@link BeanDefinitionReader} implementation to use when processing
     * resources specified via the {@link #value} attribute.
     * <p>By default, the reader will be adapted to the resource path specified:
     * {@code ".groovy"} files will be processed with a
     * {@link org.springframework.beans.factory.groovy.GroovyBeanDefinitionReader GroovyBeanDefinitionReader};
     * whereas, all other resources will be processed with an
     * {@link org.springframework.beans.factory.xml.XmlBeanDefinitionReader XmlBeanDefinitionReader}.
     * @see #value
     */
    Class<? extends BeanDefinitionReader> reader() default BeanDefinitionReader.class;

}
```

## 解析

我们可以使用这个注解导入 XML 配置。我们可以用 `locations` 参数指定 XML 文件的位置，或者用它的别名 `value` 参数指定:

```java
@Configuration
@ImportResource("classpath:/annotations.xml")
class VehicleFactoryConfig {}
```



