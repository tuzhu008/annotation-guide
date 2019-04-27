# @Validated

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Validated {

	/**
	 * Specify one or more validation groups to apply to the validation step
	 * kicked off by this annotation.
	 * <p>JSR-303 defines validation groups as custom annotations which an application declares
	 * for the sole purpose of using them as type-safe group arguments, as implemented in
	 * {@link org.springframework.validation.beanvalidation.SpringValidatorAdapter}.
	 * <p>Other {@link org.springframework.validation.SmartValidator} implementations may
	 * support class arguments in other ways as well.
	 */
	Class<?>[] value() default {};

}

```

## 解析



