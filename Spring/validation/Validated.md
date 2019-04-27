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

`@Validated`  注解是 JSR-303 [`Valid`](https://docs.oracle.com/javaee/7/api/javax/validation/Valid.html?is-external=true) 的变体，支持验证组的规范。设计用于方便地使用 Spring 的 JSR-303 支持，但不是特定于JSR-303。

例如，可以与 Spring MVC 处理器方法参数一起使用。支持 [`SmartValidator`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/validation/SmartValidator.html) 的验证提示概念，验证组类充当提示对象。

还可以与方法级验证一起使用，这表明应该在方法级验证特定的类\(作为对应的验证拦截器的切入点\)，但也可以选择在带注解的类中指定方法级验证的验证组。在方法级别应用此注解允许覆盖特定方法的验证组，但不作为切入点；不过，要触发特定 bean 的方法验证，首先需要一个类级别的注解。还可以用作自定义原型注解或自定义特定于组的验证注解上的元注解。

只要类路径上有 JSR-303 实现\(如 Hibernate 验证器\)，Bean validation 1.1 支持的方法验证特性就会自动启用。这使得 bean 方法可以在其参数和/或返回值上使用 `javax.validation` 约束进行注解。带有此类注解的方法的目标类需要在类型级别上使用 `@Validated` 注解，以便搜索它们的方法来搜索内联约束注解。

