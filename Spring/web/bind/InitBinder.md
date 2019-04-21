# @InitBinder

## 定义

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface InitBinder {

    /**
     * The names of command/form attributes and/or request parameters
     * that this init-binder method is supposed to apply to.
     * <p>Default is to apply to all command/form attributes and all request parameters
     * processed by the annotated handler class. Specifying model attribute names or
     * request parameter names here restricts the init-binder method to those specific
     * attributes/parameters, with different init-binder methods typically applying to
     * different groups of attributes or parameters.
     */
    String[] value() default {};

}
```

## 解析

`@InitBinder` 用于标识初始化 [WebDataBinder](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/WebDataBinder.html) 的方法，WebDataBinder 将用于填充带注释的处理程序方法的命令和表单对象参数。

这种init-binder方法支持RequestMapping支持的所有参数，但命令/表单对象和相应的验证结果对象除外。Init-binder方法必须没有返回值;它们通常被声明为无效。

典型的参数是WebDataBinder与WebRequest或Locale结合使用，允许注册特定于上下文的编辑器。

