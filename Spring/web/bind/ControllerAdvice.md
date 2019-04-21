# @ControllerAdvice

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface ControllerAdvice {

    /**
     * Alias for the {@link #basePackages} attribute.
     * <p>Allows for more concise annotation declarations e.g.:
     * {@code @ControllerAdvice("org.my.pkg")} is equivalent to
     * {@code @ControllerAdvice(basePackages="org.my.pkg")}.
     * @since 4.0
     * @see #basePackages()
     */
    @AliasFor("basePackages")
    String[] value() default {};

    /**
     * Array of base packages.
     * <p>Controllers that belong to those base packages or sub-packages thereof
     * will be included, e.g.: {@code @ControllerAdvice(basePackages="org.my.pkg")}
     * or {@code @ControllerAdvice(basePackages={"org.my.pkg", "org.my.other.pkg"})}.
     * <p>{@link #value} is an alias for this attribute, simply allowing for
     * more concise use of the annotation.
     * <p>Also consider using {@link #basePackageClasses()} as a type-safe
     * alternative to String-based package names.
     * @since 4.0
     */
    @AliasFor("value")
    String[] basePackages() default {};

    /**
     * Type-safe alternative to {@link #value()} for specifying the packages
     * to select Controllers to be assisted by the {@code @ControllerAdvice}
     * annotated class.
     * <p>Consider creating a special no-op marker class or interface in each package
     * that serves no purpose other than being referenced by this attribute.
     * @since 4.0
     */
    Class<?>[] basePackageClasses() default {};

    /**
     * Array of classes.
     * <p>Controllers that are assignable to at least one of the given types
     * will be assisted by the {@code @ControllerAdvice} annotated class.
     * @since 4.0
     */
    Class<?>[] assignableTypes() default {};

    /**
     * Array of annotations.
     * <p>Controllers that are annotated with this/one of those annotation(s)
     * will be assisted by the {@code @ControllerAdvice} annotated class.
     * <p>Consider creating a special annotation or use a predefined one,
     * like {@link RestController @RestController}.
     * @since 4.0
     */
    Class<? extends Annotation>[] annotations() default {};

}
```

## 解析

定制化的 `@Component` ，用于声明要在多个 `@Controller` 类之间共享的 `@ExceptionHandler`、`@InitBinder` 或`@ModelAttribute` 方法的类。

带有 `@ControllerAdvice` 的类可以显式声明为 Spring bean，也可以通过类路径扫描自动检测。所有这些 bean 都是通过[AnnotationAwareOrderComparator](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/core/annotation/AnnotationAwareOrderComparator.html) 进行排序的，即基于 `@Order` 和 [Ordered](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/core/Ordered.html)，并在运行时按该顺序应用。对于处理异常，`@ExceptionHandler` 将在第一个具有匹配异常处理器方法的 advice 中选择。对于模型属性和 InitBinder 初始化，`@ModelAttribute` 和 `@InitBinder` 方法也将遵循 `@ControllerAdvice` 顺序。

**注意**：对于 `@ExceptionHandler` 方法，在特定 advice bean 的处理程序方法中，根异常匹配将优先于仅匹配当前异常原因。然而，高优先级 advice 上的原因匹配仍然比低优先级 advice bean上的任何匹配\(无论是根级别还是原因级别\)更受欢迎。因此，请使用相应的顺序在区分优先的 advice bean 上声明主根异常映射!

默认情况下，@ControllerAdvice中的方法全局应用于所有控制器。使用选择器annotation\(\)、basepackageclass\(\)和basePackages\(\)\(或其别名值\(\)\)来定义更窄的目标控制器子集。如果声明了多个选择器，或者应用了逻辑，这意味着所选控制器应该至少匹配一个选择器。注意，选择器检查是在运行时执行的，因此添加许多选择器可能会对性能产生负面影响，并增加复杂性。

