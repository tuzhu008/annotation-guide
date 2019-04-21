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



