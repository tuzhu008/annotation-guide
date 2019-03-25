# @PropertySource

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Repeatable(PropertySources.class)
public @interface PropertySource {

    /**
     * Indicate the name of this property source. If omitted, a name will
     * be generated based on the description of the underlying resource.
     * @see org.springframework.core.env.PropertySource#getName()
     * @see org.springframework.core.io.Resource#getDescription()
     */
    String name() default "";

    /**
     * Indicate the resource location(s) of the properties file to be loaded.
     * <p>Both traditional and XML-based properties file formats are supported
     * &mdash; for example, {@code "classpath:/com/myco/app.properties"}
     * or {@code "file:/path/to/file.xml"}.
     * <p>Resource location wildcards (e.g. *&#42;/*.properties) are not permitted;
     * each location must evaluate to exactly one {@code .properties} resource.
     * <p>${...} placeholders will be resolved against any/all property sources already
     * registered with the {@code Environment}. See {@linkplain PropertySource above}
     * for examples.
     * <p>Each location will be added to the enclosing {@code Environment} as its own
     * property source, and in the order declared.
     */
    String[] value();

    /**
     * Indicate if failure to find the a {@link #value() property resource} should be
     * ignored.
     * <p>{@code true} is appropriate if the properties file is completely optional.
     * Default is {@code false}.
     * @since 4.0
     */
    boolean ignoreResourceNotFound() default false;

    /**
     * A specific character encoding for the given resources, e.g. "UTF-8".
     * @since 4.3
     */
    String encoding() default "";

    /**
     * Specify a custom {@link PropertySourceFactory}, if any.
     * <p>By default, a default factory for standard resource files will be used.
     * @since 4.3
     * @see org.springframework.core.io.support.DefaultPropertySourceFactory
     * @see org.springframework.core.io.support.ResourcePropertySource
     */
    Class<? extends PropertySourceFactory> factory() default PropertySourceFactory.class;

}
```

## 解析

`@PropertySource` 注解对添加一个 PropertySource 到 Spring 的环境变量中提供了一个便捷的和声明式的机制。

通过这个注解，我们可以为应用程序设置定义属性文件:

```java
@Configuration
@PropertySource("classpath:/annotations.properties")
class VehicleFactoryConfig {}
```

`@PropertySource` 利用了Java 8 的重复注解特性，这意味着我们可以用它多次标记一个类:

```java
@Configuration
@PropertySource("classpath:/annotations.properties")
@PropertySource("classpath:/vehicle-factory.properties")
class VehicleFactoryConfig {}
```

给出一个名为 ”app.properties” 的文件，它含了 `testbean.name=myTestBean` 的键值对，下面的 `@Configuration` 类使用`@PropertySource` 的方式来调用 `testBean.getName()`，将会返回 ”myTestBean”。

```java
@Configuration
@PropertySource("classpath:/com/myco/app.properties")
public class AppConfig {
 @Autowired
 Environment env;

 @Bean
 public TestBean testBean() {
  TestBean testBean = new TestBean();
  testBean.setName(env.getProperty("testbean.name"));
  return testBean;
 }
}
```

任何出现在 `@PropertySource` 中的资源位置占位符都会被注册在环境变量中的资源解析。

```
@Configuration
@PropertySource("classpath:/com/${my.placeholder:default/path}/app.properties")
public class AppConfig {
 @Autowired
 Environment env;

 @Bean
 public TestBean testBean() {
  TestBean testBean = new TestBean();
  testBean.setName(env.getProperty("testbean.name"));
  return testBean;
 }
}
```

  


