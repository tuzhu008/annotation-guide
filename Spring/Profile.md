# @Profile

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional(ProfileCondition.class)
public @interface Profile {

    /**
     * The set of profiles for which the annotated component should be registered.
     */
    String[] value();

}
```

## 解析

如果我们希望 Spring 仅在特定配置文件处于活动状态时才使用 `@Component` 类或 `@Bean` 方法，我们可以用 `@Profile` 标记它。我们可以使用注释的 `value` 参数来配置配置文件的名称:

```java
@Component
@Profile("sportDay")
class Bike implements Vehicle {}
```

## 详述

Profiles 是框架的一个核心特性—它允许我们将 bean 映射到不同的概要文件—例如，dev、test、prod。

然后，我们可以在不同的环境中激活不同的配置文件来引导我们需要的bean:

