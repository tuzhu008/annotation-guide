# @SessionAttributes

## 定义

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface SessionAttributes {

    /**
     * Alias for {@link #names}.
     */
    @AliasFor("names")
    String[] value() default {};

    /**
     * The names of session attributes in the model that should be stored in the
     * session or some conversational storage.
     * <p><strong>Note</strong>: This indicates the <em>model attribute names</em>.
     * The <em>session attribute names</em> may or may not match the model attribute
     * names. Applications should therefore not rely on the session attribute
     * names but rather operate on the model only.
     * @since 4.2
     */
    @AliasFor("value")
    String[] names() default {};

    /**
     * The types of session attributes in the model that should be stored in the
     * session or some conversational storage.
     * <p>All model attributes of these types will be stored in the session,
     * regardless of attribute name.
     */
    Class<?>[] types() default {};

}
```

## 解析

`@SessionAttributes` 注解用于表示特定处理器使用的会话属性。

`@SessionAttributes` 用于在会话中存储 Model 的属性，一般作用在类的级别。像下面的代码，model 的属性 user 会被存储到session 中，因为 `@ModelAttribute` 与 `@SessionAttributes` 有相同的注解。

```java
@Controller
@SessionAttributes("user")
public class ModelController {

    @ModelAttribute("user")
    public User initUser(){
        User user = new User();
        user.setName("default");
        return user;
    }
}
```



