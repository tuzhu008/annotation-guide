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



