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
     * Resource locations from which to import.
     * <p>Supports resource-loading prefixes such as {@code classpath:},
     * {@code file:}, etc.
     * <p>Consult the Javadoc for {@link #reader} for details on how resources
     * will be processed.
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



