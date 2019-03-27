# @ConfigurationProperties

## 定义

```java
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ConfigurationProperties {

    /**
     * 绑定到此对象的有效属性的名称前缀。
     * {@link #prefix()} 的同义词。 
     * 一个有效的前缀是由一个或多个用 . 分隔的单词定义的(例如. {@code "acme.system.feature"})
     * @return 要绑定的属性的名称前缀
     */
    @AliasFor("prefix")
    String value() default "";

    /**
     * The name prefix of the properties that are valid to bind to this object. Synonym
     * for {@link #value()}. A valid prefix is defined by one or more words separated with
     * dots (e.g. {@code "acme.system.feature"}).
     * @return the name prefix of the properties to bind
     */
    @AliasFor("value")
    String prefix() default "";

    /**
     * Flag to indicate that when binding to this object invalid fields should be ignored.
     * Invalid means invalid according to the binder that is used, and usually this means
     * fields of the wrong type (or that cannot be coerced into the correct type).
     * @return the flag value (default false)
     */
    boolean ignoreInvalidFields() default false;

    /**
     * Flag to indicate that when binding to this object unknown fields should be ignored.
     * An unknown field could be a sign of a mistake in the Properties.
     * @return the flag value (default true)
     */
    boolean ignoreUnknownFields() default true;

}
```

## 解析

外部化配置的注解。如果您想绑定和验证一些外部属性\(例如来自 `.properties` 文件\)，请将其添加到 `@Configuration` 类中的类定义或 `@Bean` 方法中。

注意，与 `@Value` 相反，SpEL 表达式不计算值，因为属性值是外部化的。

