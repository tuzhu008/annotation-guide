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

### 简单属性

官方文档建议将配置属性隔离到单独的 POJO 中。我们从这里开始:

```java
@Configuration
@PropertySource("classpath:configprops.properties")
@ConfigurationProperties(prefix = "mail")
public class ConfigProperties {

    private String hostName;
    private int port;
    private String from;

    // standard getters and setters
}
```

我们使用 `@Configuration` ，以便 Spring 在应用程序上下文中创建一个 Spring bean。

我们还使用 `@PropertySource` 来定义属性文件的位置。否则，Spring 使用默认位置\(classpath:application.properties\)。

`@ConfigurationProperties`_** 最适合使用具有相同前缀的**_**阶层式的**_**属性。所以我们添加了一个 **_`mail`_** 前缀。**_

Spring 框架使用标准的 Java bean setter，因此为每个属性声明 setter 非常重要。

注意：如果我们不在 POJO 中使用 `@Configuration`，那么我们需要在主Spring应用程序类中添加@EnableConfigurationProperties\(ConfigProperties.class\)来将属性绑定到POJO中:

### 嵌套属性

我们可以在 Lists、Maps 和 Classes 中嵌套属性。

让我们创建一个新的 `Credentials`_ _类，用于一些嵌套属性:

```java
public class Credentials {
    private String authMethod;
    private String username;
    private String password;

    // standard getters and setters
}
```

我们还更新了 ConfigProperties 类来使用 List、Map 和 `Credentials`类:

```java
public class ConfigProperties {

    private String host;
    private int port;
    private String from;
    private List<String> defaultRecipients;
    private Map<String, String> additionalHeaders;
    private Credentials credentials;

    // standard getters and setters
}
```

以下属性文件将设置所有字段:

```bash
#Simple properties
mail.hostname=mailer@mail.com
mail.port=9000
mail.from=mailer@mail.com

#List properties
mail.defaultRecipients[0]=admin@mail.com
mail.defaultRecipients[1]=owner@mail.com

#Map Properties
mail.additionalHeaders.redelivery=true
mail.additionalHeaders.secure=true

#Object properties
mail.credentials.username=john
mail.credentials.password=password
mail.credentials.authMethod=SHA1
```

**注意：**如果我们不在 POJO 中使用 `@Configuration`，那么我们需要在主 Spring 应用程序类中添加`@EnableConfigurationProperties(ConfigProperties.class)`来将属性绑定到 POJO 中:

```java
@SpringBootApplication
@EnableConfigurationProperties(ConfigProperties.class)
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

就是这样！Spring 将自动绑定我们在属性文件中定义的任何属性，这些属性具有前缀邮件和与ConfigProperties类中的一个字段相同的名称。

Spring使用一些放松的规则来绑定属性。因此，以下变量都绑定到属性主机名:

### Relaxed绑定

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
| person.first-name | 虚线表示，推荐用于 .properties 和 .yml 文件中 |
| person.first\_name | 下划线表示，用于 .properties 和 .yml 文件的可选格式 |
| PERSON\_FIRST\_NAME | 大写形式，使用系统环境变量时推荐 |

### 属性转换

将外部应用配置绑定到`@ConfigurationProperties`beans时，Spring 会尝试将属性强制转换为正确的类型。如果需要自定义类型转换器，你可以提供一个`ConversionService`bean（bean id 为`conversionService`），或自定义属性编辑器（通过`CustomEditorConfigurer`bean），或自定义`Converters`（bean定义时需要注解`@ConfigurationPropertiesBinding`）。

**注：**由于该 bean 在应用程序生命周期的早期就需要使用，所以确保限制你的`ConversionService`使用的依赖。通常，在创建时期任何你需要的依赖可能都没完全初始化。

### 校验

Spring Boot 将尝试校验外部配置，默认使用 JSR-303（如果在 classpath 路径中），你只需要将 JSR-303 `javax.validation`约束注解添加到`@ConfigurationProperties`类上：

```java
@ConfigurationProperties(prefix="connection")
public class ConnectionProperties {

    @NotNull
    private InetAddress remoteAddress;

    // ... getters and setters

}
```

为了校验内嵌属性的值，你需要使用 `@Valid`注解关联的字段以触发它的校验，例如：

```java
@ConfigurationProperties(prefix="connection")
public class ConnectionProperties {

    @NotNull
    @Valid
    private RemoteAddress remoteAddress;

    // ... getters and setters

    public static class RemoteAddress {

        @NotEmpty
        public String hostname;

        // ... getters and setters

    }

}
```

你也可以通过创建一个叫做`configurationPropertiesValidator`的 bean 来添加自定义的 Spring`Validator`。`@Bean`方法需要声明为`static`，因为配置属性校验器在应用程序生命周期中创建的比较早，将`@Bean`方法声明为`static`允许该 bean 在创建时不需要实例化`@Configuration`类，从而避免了早期实例化（early instantiation）的所有问题。相关的示例可以看[这里](https://github.com/spring-projects/spring-boot/tree/v1.4.1.RELEASE/spring-boot-samples/spring-boot-sample-property-validation)。

**注**`spring-boot-actuator`模块包含一个暴露所有`@ConfigurationProperties`beans的端点（endpoint），通过浏览器打开`/configprops`进行浏览，或使用等效的JMX端点，具体参考[Production ready features](https://qbgbook.gitbooks.io/spring-boot-reference-guide-zh/content/V. Spring Boot Actuator/40. Endpoints.md)。

### @ConfigurationProperties vs. @Value {#2475-configurationproperties-vs-value}

`@Value`是 Spring 容器的一个核心特性，它没有提供跟 type-safe Configuration Properties 相同的特性。下面的表格总结了

`@ConfigurationProperties`和`@Value、`支持的特性：

| 特性 | @ConfigurationProperties | @Value |
| :--- | :---: | :---: |
| Relaxed绑定 | Yes | No |
| Meta-data支持 | Yes | No |
| SpEL表达式 | No | Yes |

如果你为自己的组件定义了一系列的配置 keys，我们建议你将它们以`@ConfigurationProperties`注解的 POJO 进行分组。由于`@Value`不支持 relaxed 绑定，所以如果你使用环境变量提供属性值的话，它就不是很好的选择。最后，尽管 `@Value`可以写 `SpEL`表达式，但这些表达式不会处理来自 [Application属性文件](http://docs.spring.io/spring-boot/docs/1.4.1.RELEASE/reference/htmlsingle/#boot-features-external-config-application-property-files) 的属性。

