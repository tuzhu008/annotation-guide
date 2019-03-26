# @SpringBootConfiguration

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {

}
```

## 解析

`@SpringBootConfiguration` 继承自 `@Configuration` 二者功能也一致，标注当前类是配置类，并会将当前类内声明的一个或多个以 `@Bean` 注解标记的方法的实例纳入到 spring 容器中，并且实例名就是方法名。  


`@SpringBootConfiguration `可以作为 Spring 的标准 `@Configuration` 注解的替代，以便自动找到配置\(例如在测试中\)。

应用程序应该只包含一个 `@SpringBootConfiguration`，大多数惯用的  Spring Boot 应用程序将从`@ SpringBootApplication` 继承它。

