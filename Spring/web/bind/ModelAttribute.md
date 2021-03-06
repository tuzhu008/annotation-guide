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

`@ModelAttribute` 注解将方法参数或方法返回值绑定到一个命名的模型属性，然后将其暴露给 web 视图。

在下面的示例中，我们将通过一个共同的概念来演示注解的可用性和功能：公司员工提交的表单。

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

在本文的后面，我们将看到一个完整的示例，演示如何使用 employee 对象填充 employeeView 模板。

因此，它将表单数据与 bean 绑定。使用 `@RequestMapping` 注解的控制器可以使用 `@ModelAttribute` 注解自定义类参数。

这就是 Spring-MVC 中通常所说的数据绑定，这是一种通用的机制，可以让您不必逐个解析每个表单字段。

## 实例

在本节中，我们将提供一个示例：一个非常基本的表单，它提示用户\(在我们的特定示例中是公司的员工\)输入一些个人信息\(特别是姓名和id\)。在提交完成且没有任何错误之后，用户希望看到以前提交的数据显示在另一个屏幕上。

### 视图

让我们首先创建一个简单的表单，其中包含 `id` 和 `name` 字段：

```html
<form:form method="POST" action="/spring-mvc-java/addEmployee"
  modelAttribute="employee">
    <form:label path="name">Name</form:label>
    <form:input path="name" />

    <form:label path="id">Id</form:label>
    <form:input path="id" />

    <input type="submit" value="Submit" />
</form:form>
```

### **Model**

如前所述，模型对象非常简单，包含“前端”属性所需的所有内容。现在，让我们来看一个例子:

```java
@XmlRootElement
public class Employee {

    private long id;
    private String name;

    public Employee(long id, String name) {
        this.id = id;
        this.name = name;
    }

    // standard getters and setters removed
}
```

### **Controller**

这是控制器类，实现了上述视图的逻辑：

```java
@Controller
@ControllerAdvice
public class EmployeeController {

    private Map<Long, Employee> employeeMap = new HashMap<>();

    @RequestMapping(value = "/addEmployee", method = RequestMethod.POST)
    public String submit(
      @ModelAttribute("employee") Employee employee,
      BindingResult result, ModelMap model) {
        if (result.hasErrors()) {
            return "error";
        }
        model.addAttribute("name", employee.getName());
        model.addAttribute("id", employee.getId());

        employeeMap.put(employee.getId(), employee);

        return "employeeView";
    }

    @ModelAttribute
    public void addAttributes(Model model) {
        model.addAttribute("msg", "Welcome to the Netherlands!");
    }
}
```

在 `submit()` 方法中，我们有一个 Employee 对象绑定到我们的视图。

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    姓名：<p th:text="${employee.name}"></p>
    id：<p th:text="${employee.id}"></p>
</body>
</html>
```

您可以简单地将表单字段映射到对象模型。在该方法中，我们从表单中获取值并将其设置为 `ModelMap`。也就是说，我们可以在视图中使用：

```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    姓名：<p th:text="${name}"></p>
    id：<p th:text="${id}"></p>
</body>
</html>
```

此外，还有一个 `addAttributes()` 方法。它的目的是在模型中添加将被全局标识的值。也就是说，对于每个控制器方法的每个请求，都会返回一个默认值作为响应。我们还**必须**将特定的类标注为 `@ControllerAdvice`。

```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    msg：<p th:text="${msg}"></p>
</body>
</html>
```

### 



