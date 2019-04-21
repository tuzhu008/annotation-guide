# @ExceptionHandler

## 定义

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ExceptionHandler {

	/**
	 * Exceptions handled by the annotated method. If empty, will default to any
	 * exceptions listed in the method argument list.
	 */
	Class<? extends Throwable>[] value() default {};

}

```

## 解析



