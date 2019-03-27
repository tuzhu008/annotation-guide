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

`@ConfigurationProperties`不仅可以注解在类上，也可以注解在public `@Bean`方法上，当你需要为不受控的第三方组件绑定属性时，该方法将非常有用。

为了从`Environment`属性中配置一个bean，你需要使用`@ConfigurationProperties`注解该 bean：

```java
@ConfigurationProperties(prefix = "foo")
@Bean
public FooComponent fooComponent() {
    ...
}
```

所有以`foo`为前缀的属性定义都会被映射到`FooComponent`上。

### 匹配规则

Spring Boot将`Environment`属性绑定到`@ConfigurationProperties`beans时会使用一些宽松的规则，所以`Environment`属性名和bean属性名不需要精确匹配。常见的示例中有用的包括虚线分割（比如，`context-path`绑定到`contextPath`），将environment属性转为大写字母（比如，`PORT`绑定`port`）。

例如，给定以下`@ConfigurationProperties`类：

```java
@ConfigurationProperties(prefix="person")
public class OwnerProperties {

    private String firstName;

    public String getFirstName() {
        return this.firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

}
```

下面的属性名都能使用：

| 属性 | 说明 |
| :--- | :--- |
| person.firstName | 标准驼峰规则 |
|  |  |
|  |  |
|  |  |



