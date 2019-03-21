# @Value

## 定义

```java
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Value {

	/**
	 * The actual value expression: for example {@code #{systemProperties.myProp}}.
	 */
	String value();

}
```

## 解析





