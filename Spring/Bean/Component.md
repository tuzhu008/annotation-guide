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



