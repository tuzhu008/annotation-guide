# @Conditional

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Conditional {

    /**
     * All {@link Condition Conditions} that must {@linkplain Condition#matches match}
     * in order for the component to be registered.
     */
    Class<? extends Condition>[] value();

}
```

## 解析

`@Conditional` 注解表示只有当所有条件都匹配时，组件才有资格注册。

条件是在注册 bean 定义之前可以通过编程确定的任何状态，参见 [Condition](/Spring/docs/Condition.md)。

`@Conditional` 注释可以下列任何一种方式使用:

* 作为任何直接或间接用 `@Component`注解的类的类型级别注解，包括 `@Configuration` 类

* 作为元注解，用于组合自定义 stereotype 注释

* 作为任何 `@Bean`方法的方法级注解

如果 `@Configuration` 类被标记为 `@Conditional`，那么与该类关联的所有 `@Bean` 方法、`@Import` 注解和 `@ComponentScan`注解都将受这些条件的约束。

**注意：**不支持 `@Configuration` 注释的继承；不考虑来自超类或覆盖方法的任何条件。为了加强这些语义，`@Conditional` 本身没有声明为 `@inherit`；此外，任何使用 `@Conditional` 进行元注释的自定义复合注注解都不能声明为 `@ inherit`。

