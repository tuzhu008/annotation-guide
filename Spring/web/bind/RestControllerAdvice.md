# @RestControllerAdvice

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@ControllerAdvice
@ResponseBody
public @interface RestControllerAdvice {

    /**
     * Alias for the {@link #basePackages} attribute.
     * <p>Allows for more concise annotation declarations e.g.:
     * {@code @ControllerAdvice("org.my.pkg")} is equivalent to
     * {@code @ControllerAdvice(basePackages="org.my.pkg")}.
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
     */
    @AliasFor("value")
    String[] basePackages() default {};

    /**
     * Type-safe alternative to {@link #value()} for specifying the packages
     * to select Controllers to be assisted by the {@code @ControllerAdvice}
     * annotated class.
     * <p>Consider creating a special no-op marker class or interface in each package
     * that serves no purpose other than being referenced by this attribute.
     */
    Class<?>[] basePackageClasses() default {};

    /**
     * Array of classes.
     * <p>Controllers that are assignable to at least one of the given types
     * will be assisted by the {@code @ControllerAdvice} annotated class.
     */
    Class<?>[] assignableTypes() default {};

    /**
     * Array of annotations.
     * <p>Controllers that are annotated with this/one of those annotation(s)
     * will be assisted by the {@code @ControllerAdvice} annotated class.
     * <p>Consider creating a special annotation or use a predefined one,
     * like {@link RestController @RestController}.
     */
    Class<? extends Annotation>[] annotations() default {};

}
```

## 解析

一个方便的注解，它本身由 `@ControllerAdvice` 和 `@ResponseBody` 注释组成。

携带此注解的类型被视为控制器通知，其中 `@ExceptionHandler` 方法默认假定 `@ResponseBody` 语义。

**注意**：如果配置了适当的 HandlerMapping-HandlerAdapter 对\(如 MVC Java 配置和 MVC 名称空间中的缺省值RequestMappingHandlerMapping-RequestMappingHandlerAdapter 对\)，`@RestControllerAdvice` 将被处理。

