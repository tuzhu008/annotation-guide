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

指示类提供 Spring Boot 应用程序 `@Configuration`。可以作为 Spring 的标准 `@Configuration` 注解的替代，以便自动找到配置\(例如在测试中\)。

应用程序应该只包含一个 `@SpringBootConfiguration`，大多数惯用的Spring引导应用程序将从@SpringBootApplication继承它。

