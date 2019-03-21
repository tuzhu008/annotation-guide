### _@DependsOn_ {#depends-on}

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface DependsOn {

    String[] value() default {};

}
```

## 解析

我们可以使用这个注解使 Spring 在被注释的 bean 之前初始化其他 bean。通常，这种行为是自动的，基于 bean 之间显式的依赖关系。

只有当依赖项是隐式的时，我们才需要这个注释，例如，加载 JDBC 驱动程序或静态变量初始化。

我们可以在指定依赖 bean 名称的依赖类上使用 `@DependsOn`。注解的 `value` 参数需要一个包含依赖 bean 名称的数组:

