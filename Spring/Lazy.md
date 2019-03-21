# @Lazy

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.CONSTRUCTOR, ElementType.PARAMETER, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Lazy {

    /**
     * Whether lazy initialization should occur.
     */
    boolean value() default true;

}
```

## 解析

当我们想要惰性地初始化 bean 时，我们使用 `@Lazy`。默认情况下，Spring 在应用程序上下文的启动/引导时急切地创建所有**单例 **bean。

但是，在某些情况下，我们需要在请求 bean 时创建它，而不是在应用程序启动时。

这个注解的行为取决于我们将它精确地放在哪里。我们可以这样写:

