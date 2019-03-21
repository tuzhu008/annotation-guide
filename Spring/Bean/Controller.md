# @Controller

## 定义

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {

    /**
     * The value may indicate a suggestion for a logical component name,
     * to be turned into a Spring bean in case of an autodetected component.
     * @return the suggested component name, if any (or empty String otherwise)
     */
    @AliasFor(annotation = Component.class)
    String value() default "";

}
```

## 解析

`@Controller` 是一个类级注解，它告诉 Spring 框架这个类在 Spring MVC 中充当 **controller**:

```java
@Controller
public class VehicleController {
    // ...
}
```



