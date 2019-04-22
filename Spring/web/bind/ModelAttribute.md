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

还可以通过在带有 `@RequestMapping` 方法的控制器类中注解访问器方法，将引用数据公开给 web 视图。允许这样的访问器方法具有 `@RequestMapping` 方法支持的任何参数，返回要公开的模型属性值。

但是请注意，当请求处理导致异常时，web 视图不能使用引用数据和所有其他模型内容，因为异常可以在任何时候引发，从而使模型的内容变得不可靠。因此 `@ExceptionHandler` 方法不提供对模型参数的访问。

## 详解

`@ModelAttribute`是 Spring-MVC 最重要的注解之一。

`@ModelAttribute` 注解将方法参数或方法返回值绑定到一个命名的模型属性，然后将其公开给web视图。

在下面的示例中，我们将通过一个共同的概念来演示注解的可用性和功能:公司员工提交的表单。

### 深入 _**@ModelAttribute**_

正如介绍性段落所揭示的，`@ModelAttribute` 既可以用作方法参数，也可以用于方法级别。

#### 方法级

当在方法级别使用注释时，它表明该方法的目的是添加一个或多个模型属性。这些方法支持与 `@RequestMapping` 方法相同的参数类型，但不能直接映射到请求。

让我们来看一个快速的例子，开始了解它是如何工作的:

```java
@ModelAttribute
public void addAttributes(Model model) {
    model.addAttribute("msg", "Welcome to the Netherlands!");
}
```

在本例中，我们展示了一个方法，它将一个名为 `msg` 的属性添加到控制器类中定义的所有模型中。

当然，我们将在稍后的文章中看到这一点。

通常，Spring-MVC 总是在调用任何请求处理器方法之前首先调用该方法。也就是说，**在调用带有 **`@RequestMapping`** 注解的控制器方法之前调用 **`@ModelAttribute`** 方法**。序列背后的逻辑是，必须在控制器方法内的任何处理开始之前创建模型对象。

同样重要的是，要将相应的类注解为 `@ControllerAdvice`。因此，您可以在模型中添加将被标识为全局的值。这实际上意味着对于每个请求，对于响应部分中的每个方法，都存在一个默认值。

#### 方法参数

当用作方法参数时，它指示应该从模型中检索参数。当不存在时，应该首先实例化它，然后将其添加到模型中，一旦出现在模型中，就应该从所有具有匹配名称的请求参数填充 arguments 字段。

在 employee model 属性后面的代码片段中，填充了提交给 addEmployee 端点的表单中的数据。在调用 submit 方法之前，Spring MVC 会在后台执行以下操作:

```java
@RequestMapping(value = "/addEmployee", method = RequestMethod.POST)
public String submit(@ModelAttribute("employee") Employee employee) {
    // Code that uses the employee object

    return "employeeView";
}
```

在本文的后面，我们将看到一个完整的示例，演示如何使用employee对象填充employeeView模板。



因此，它将表单数据与bean绑定。使用@RequestMapping注释的控制器可以使用@ModelAttribute注释自定义类参数。



这就是Spring-MVC中通常所说的数据绑定，这是一种通用的机制，可以让您不必逐个解析每个表单字段。

