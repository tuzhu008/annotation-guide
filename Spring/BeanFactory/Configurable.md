# @Configurable

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Configurable {

	/**
	 * The name of the bean definition that serves as the configuration template.
	 */
	String value() default "";

	/**
	 * Are dependencies to be injected via autowiring?
	 */
	Autowire autowire() default Autowire.NO;

	/**
	 * Is dependency checking to be performed for configured objects?
	 */
	boolean dependencyCheck() default false;

	/**
	 * Are dependencies to be injected prior to the construction of an object?
	 */
	boolean preConstruction() default false;

}

```



