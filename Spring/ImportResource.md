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
     * {@link BeanDefinitionReader} 实现用于在处理通过 {@link #value} 属性指定的资源时使用的。
     * <p>默认情况下，reader 将适应指定的资源路径:
     * {@code ".groovy"} 文件将用一个
     * {@link org.springframework.beans.factory.groovy.GroovyBeanDefinitionReader GroovyBeanDefinitionReader}
     * 处理;
     * 然而，所有其他资源都将使用
     * {@link org.springframework.beans.factory.xml.XmlBeanDefinitionReader XmlBeanDefinitionReader} 处理.
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



