# @RestController

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Controller
@ResponseBody
public @interface RestController {

    /**
     * The value may indicate a suggestion for a logical component name,
     * to be turned into a Spring bean in case of an autodetected component.
     * @return the suggested component name, if any (or empty String otherwise)
     * @since 4.0.1
     */
    @AliasFor(annotation = Controller.class)
    String value() default "";

}
```

## 解析

`@RestController` 是一个专门版本的控制器。它包括 [`@Controller`](/Spring/stereotype/Controller.md) 和 [`@ResponseBody`](/Spring/web/bind/ResponseBody.md) 注解，因此简化了控制器实现。

携带此注解的类型被视为控制器，其中的 `@RequestMapping` 方法默认假定 `@ResponseBody` 语义。

**注意**：如果配置了适当的 HandlerMapping-HandlerAdapter 对\(如 MVC Java 配置和 MVC 名称空间中的缺省值RequestMappingHandlerMapping-RequestMappingHandlerAdapter 对\)，`@RestController` 将被处理。

```java
@RestController
@RequestMapping("books-rest")
public class SimpleBookRestController {
     
    @GetMapping("/{id}", produces = "application/json")
    public Book getBook(@PathVariable int id) {
        return findBookById(id);
    }
 
    private Book findBookById(int id) {
        // ...
    }
}
```



