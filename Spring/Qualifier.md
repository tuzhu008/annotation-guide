### _@Qualifier_ {#qualifier}

## 定义

```java
public @interface Qualifier {

    String value() default "";

}
```

## 解析

我们使用 `@Qualifier` 和 `@Autowired` 来提供 bean id 或 bean 名称，以便在模糊的情况下使用。

例如，下面两个 bean 实现了相同的接口:

```java
class Bike implements Vehicle {}

class Car implements Vehicle {}
```

如果 Spring 需要注入一个 Vehicle bean ，它最终会有多个匹配的定义。在这种情况下，我们可以使用 `@Qualifier` 注解显式地提供bean 的名称。

使用构造函数注入:

