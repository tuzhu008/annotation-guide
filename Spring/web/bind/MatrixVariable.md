# @MatrixVariable

## 定义

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MatrixVariable {

    /**
     * Alias for {@link #name}.
     */
    @AliasFor("name")
    String value() default "";

    /**
     * The name of the matrix variable.
     * @since 4.2
     * @see #value
     */
    @AliasFor("value")
    String name() default "";

    /**
     * The name of the URI path variable where the matrix variable is located,
     * if necessary for disambiguation (e.g. a matrix variable with the same
     * name present in more than one path segment).
     */
    String pathVar() default ValueConstants.DEFAULT_NONE;

    /**
     * Whether the matrix variable is required.
     * <p>Default is {@code true}, leading to an exception being thrown in
     * case the variable is missing in the request. Switch this to {@code false}
     * if you prefer a {@code null} if the variable is missing.
     * <p>Alternatively, provide a {@link #defaultValue}, which implicitly sets
     * this flag to {@code false}.
     */
    boolean required() default true;

    /**
     * The default value to use as a fallback.
     * <p>Supplying a default value implicitly sets {@link #required} to
     * {@code false}.
     */
    String defaultValue() default ValueConstants.DEFAULT_NONE;

}
```

## 解析

`@MatrixVariable` 注解用于指示方法参数应绑定到路径段中的名称-值对。支持 RequestMapping 带该注解的处理器方法。

如果方法参数类型为 Map，并且指定了一个矩阵变量名，那么假设有合适的转换策略，则将矩阵变量值转换为 Map。

如果方法参数是 `Map<String、String>` 或 `MultiValueMap<String、String>` 且变量名未指定，则映射将填充所有矩阵变量名和值。

## 用法

### 配置

要启用 Spring MVC 矩阵变量，让我们从配置开始：

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void configurePathMatch(PathMatchConfigurer configurer) {
        UrlPathHelper urlPathHelper = new UrlPathHelper();
        urlPathHelper.setRemoveSemicolonContent(false);
        configurer.setUrlPathHelper(urlPathHelper);
    }
}
```

否则，它们在默认情况下是禁用的。

### 如何使用 Matrix Variables

这些变量可以出现在路径的任何部分，使用字符 等于\(`=`\)给出值，使用分号\(`;`\)分隔每个矩阵变量。在同一路径上，我们还可以重复相同的变量名，或者使用字符逗号\(`,`\)分隔不同的值。

我们的示例中有一个控制器，它提供关于员工的信息。每个员工都有一个工作区，我们可以根据该属性进行搜索。下列查询可用于搜寻:

[http://localhost:8080/spring-mvc-java/employeeArea/workingArea=rh,informatics,admin](http://localhost:8080/spring-mvc-java/employeeArea/workingArea=rh,informatics,admin)

当我们想在 Spring MVC 中引用这些变量时，我们应该使用 `@MatrixVariable` 注解。

在我们的例子中，我们将使用 Employee 类:

```java
public class Employee {
 
    private long id;
    private String name;
    private String contactNumber;
 
    // standard setters and getters 
}
```

公司类别:

```java
public class Company {
 
    private long id;
    private String name;
 
    // standard setters and getters
}
```

这两个类将用来绑定请求参数。

### 定义矩阵变量属性

我们可以为变量指定必需的或默认的属性。在下面的例子中，`contactNumber` 是必需的，所以它必须包含在我们的路径中，如下所示:

http://localhost:8080/spring-mvc-java/employeesContacts/contactNumber=223334411

处理请求的方法如下:

```java
@RequestMapping(value = "/employeesContacts/{contactNumber}", 
  method = RequestMethod.GET)
@ResponseBody
public ResponseEntity<List<Employee>> getEmployeeBycontactNumber(
  @MatrixVariable(required = true) String contactNumber) {
    List<Employee> employeesList = new ArrayList<Employee>();
    ...
    return new ResponseEntity<List<Employee>>(employeesList, HttpStatus.OK);
}
```

因此，我们将得到所有有联系电话 223334411 的员工。

```java
@RestController
public class MyController {
    @GetMapping(value = "/user/{first}/{last}",
               produces = MediaType.TEXT_PLAIN_VALUE)
    public String handler1(@MatrixVariable("first") String first,
                           @MatrixVariable("last") String last) {

        return String.format("Hello %s %s", first, last);
    }

     @GetMapping(value = "/data/{user:.*}",
            produces = MediaType.TEXT_PLAIN_VALUE)
    public String handler2(@MatrixVariable Map<String, String> data) {

        return String.format("Id: %s\nFirst name: %s\nLast Name: %s\nEmail: %s\n",
                data.get("id"), data.get("first"), data.get("last"), data.get("email"));
    }

    @GetMapping(value = "/geo/{continent}",
            produces = MediaType.TEXT_PLAIN_VALUE)
    public String handler3(@PathVariable("continent") String continent,
                           @MatrixVariable("country") String country,
                           @MatrixVariable("capital") String capital) {

        return String.format("Continent: %s\nCountry: %s\nCapital: %s\n",
                continent, country, capital);
    }
}
```

结果：

* [http://localhost:8080/user/first=John/last=Doe](http://localhost:8080/user/first=John/last=Doe)

  ```
  Hello John Doe
  ```

* [http://localhost:8080/data/id=1;first=John;last=Doe;email=johndoe@gmail.com](http://localhost:8080/data/id=1;first=John;last=Doe;email=johndoe@gmail.com)

  ```
  Id: 1
  First name: John
  Last Name: Doe
  Email: johndoe@gmail.com
  ```

* [http://localhost:8080/geo/Europe;country=Slovakia;capital=Bratislava](http://localhost:8080/geo/Europe;country=Slovakia;capital=Bratislava)

  ```
  Continent: Europe
  Country: Slovakia
  Capital: Bratislava
  ```



