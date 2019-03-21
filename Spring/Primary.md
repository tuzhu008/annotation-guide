# @Primary

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Primary {

}
```

## 解析

有时我们需要定义同一类型的多个 bean。在这些情况下，注入将不成功，因为 Spring 不知道我们需要哪个 bean。

我们已经看到了处理此场景的选项:使用 `@Qualifier` 标记所有连接点，并指定所需 bean 的名称。

然而，大多数时候我们需要一个特定的bean，很少需要其他bean。我们可以用@Primary来简化这种情况:如果我们用@Primary来标记最常用的bean，它将会被选择在不合格的注入点:



