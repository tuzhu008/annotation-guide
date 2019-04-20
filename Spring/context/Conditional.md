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
* 作为元注解，用于组合自定义原型注释



