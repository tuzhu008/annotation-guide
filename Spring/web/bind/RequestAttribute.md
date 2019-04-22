# @RequestAttribute

## 定义

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestAttribute {

    /**
     * Alias for {@link #name}.
     */
    @AliasFor("name")
    String value() default "";

    /**
     * The name of the request attribute to bind to.
     * <p>The default name is inferred from the method parameter name.
     */
    @AliasFor("value")
    String name() default "";

    /**
     * Whether the request attribute is required.
     * <p>Defaults to {@code true}, leading to an exception being thrown if
     * the attribute is missing. Switch this to {@code false} if you prefer
     * a {@code null} or Java 8 {@code java.util.Optional} if the attribute
     * doesn't exist.
     */
    boolean required() default true;

}
```

## 解析

`@RequestAttribute` 注解将方法参数绑定到请求对象的属性中。

主要动机是通过可选/必需的检查和对目标方法参数类型的转换，提供对控制器方法的请求属性的方便访问。

```java
@RequestMapping(value = "/addEmployee", method = RequestMethod.POST)
    public String submit(
            @RequestAttribute("employee")
            @ModelAttribute("employee") Employee employee,
            BindingResult result, ModelMap model, HttpServletRequest requst) {
        if (result.hasErrors()) {
            return "error";
        }
        model.addAttribute("name", employee.getName());
        model.addAttribute("id", employee.getId());

        employeeMap.put(employee.getId(), employee);

        return "employeeView";
    }
```

`@RequestAttribute("employee")` 将 Employee 的实例绑定到 `request` 的属性 `employee` 中。

