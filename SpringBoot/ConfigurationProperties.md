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
     * 绑定到此对象的有效属性的名称前缀。
     * {@link #value()} 的同义词。 
     * 一个有效的前缀是由一个或多个用 . 分隔的单词定义的(例如. {@code "acme.system.feature"})
     * @return 要绑定的属性的名称前缀
     */

    @AliasFor("value")
    String prefix() default "";

    /**
     * 一个标记，指示绑定到此对象时应忽略无效字段。
     * 根据使用的绑定器，Invalid 表示无效，
     * 通常这表示类型错误的字段(或者不能强制转换为正确的类型)。
     * @return 标记的值 (default false)
     */
    boolean ignoreInvalidFields() default false;

    /**
     * 一个标记，指示绑定到此对象时应忽略未知字段。
     * 未知字段可能是属性错误的标志。
     * @return 标记的值 (default true)
     */
    boolean ignoreUnknownFields() default true;

}
```

## 解析

外部化配置的注解。如果您想绑定和验证一些外部属性\(例如来自 `.properties` 文件\)，请将其添加到 `@Configuration` 类中的类定义或 `@Bean` 方法中。

注意，与 `@Value` 相反，SpEL 表达式不计算值，因为属性值是外部化的。

