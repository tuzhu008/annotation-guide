## _**@Component**_ {#component}

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Indexed
public @interface Component {

    /**
     * The value may indicate a suggestion for a logical component name,
     * to be turned into a Spring bean in case of an autodetected component.
     * @return the suggested component name, if any (or empty String otherwise)
     */
    String value() default "";

}
```

## 解析

`@Component` 是一个类级注释。在组件扫描期间，Spring Framework 自动检测带有 `@Component` 注解的类。

例如：

```java
@Component
class CarUtility {
    // ...
}
```

默认情况下，该类的 bean 实例具有与初始值为小写的类名称相同的名称。此外，我们可以使用该注解的可选值参数指定一个不同的名称。

由于 `@Repository`、`@Service`、`@Configuration` 和 `@Controller` 都是 `@Component` 的元注释，所以它们共享相同的bean命名行为。此外，Spring在组件扫描过程中自动拾取它们。

