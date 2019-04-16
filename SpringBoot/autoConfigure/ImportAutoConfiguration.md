## @ImportAutoConfiguration {#enable-autoconfiguration}

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Import(ImportAutoConfigurationImportSelector.class)
public @interface ImportAutoConfiguration {

    /**
     * 应该导入的自动配置类
     * 这是 {@link #classes()} 的别名.
     * @return the classes to import
     */
    @AliasFor("classes")
    Class<?>[] value() default {};

    /**
     * 应该导入的自动配置类。 
     * 当为空时，使用 {@code META-INF/spring.factories} 中的条目指定这些类，其中键是带注注解的类的完全限定名。
     * @return the classes to import
     */
    @AliasFor("value")
    Class<?>[] classes() default {};

    /**
     * Exclude specific auto-configuration classes such that they will never be applied.
     * @return the classes to exclude
     */
    Class<?>[] exclude() default {};

}
```

## 解析

导入并应用指定的自动配置类。应用与 `@EnableAutoConfiguration` 相同的排序规则，但将自动配置类限制为指定的集合，而不是咨询 `spring.factories`。

还可以使用 `exclude()` 排除特定的自动配置类，这样它们就永远不会被应用。

通常，应该优先使用 `@EnableAutoConfiguration` 而不是这个注释，但是，`@ImportAutoConfiguration` 在某些情况下非常有用，尤其是在编写测试时。

