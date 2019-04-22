# @ModelAttribute

## 定义

```java
@Target({ElementType.PARAMETER, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ModelAttribute {

    /**
     * Alias for {@link #name}.
     */
    @AliasFor("name")
    String value() default "";

    /**
     * The name of the model attribute to bind to.
     * <p>The default model attribute name is inferred from the declared
     * attribute type (i.e. the method parameter type or method return type),
     * based on the non-qualified class name:
     * e.g. "orderAddress" for class "mypackage.OrderAddress",
     * or "orderAddressList" for "List&lt;mypackage.OrderAddress&gt;".
     * @since 4.3
     */
    @AliasFor("value")
    String name() default "";

    /**
     * Allows declaring data binding disabled directly on an {@code @ModelAttribute}
     * method parameter or on the attribute returned from an {@code @ModelAttribute}
     * method, both of which would prevent data binding for that attribute.
     * <p>By default this is set to {@code true} in which case data binding applies.
     * Set this to {@code false} to disable data binding.
     * @since 4.3
     */
    boolean binding() default true;

}
```

## 解析

`@ModelAttribute`  注解将方法参数或方法返回值绑定到公开给 web 视图的命名模型属性。支持带有 `@RequestMapping`方法的控制器类。

可以使用特定的属性名，通过注解 `@RequestMapping` 方法的相应参数，将命令对象公开给 web 视图。

还可以通过在带有 `@RequestMapping` 方法的控制器类中注释访问器方法，将引用数据公开给web视图。允许这样的访问器方法具有@RequestMapping方法支持的任何参数，返回要公开的模型属性值。

但是请注意，当请求处理导致异常时，web视图不能使用引用数据和所有其他模型内容，因为异常可以在任何时候引发，从而使模型的内容变得不可靠。因此@ExceptionHandler方法不提供对模型参数的访问。

