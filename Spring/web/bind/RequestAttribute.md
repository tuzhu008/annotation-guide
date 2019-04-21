# @RequestAttribute

## 定义

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestAttribute {

	/**
	 * Alias for {@link #name}.
	 */
	@AliasFor("name")
	String value() default "";

	/**
	 * The name of the request attribute to bind to.
	 * <p>The default name is inferred from the method parameter name.
	 */
	@AliasFor("value")
	String name() default "";

	/**
	 * Whether the request attribute is required.
	 * <p>Defaults to {@code true}, leading to an exception being thrown if
	 * the attribute is missing. Switch this to {@code false} if you prefer
	 * a {@code null} or Java 8 {@code java.util.Optional} if the attribute
	 * doesn't exist.
	 */
	boolean required() default true;

}

```

## 解析



