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

@Conditional 注解表示只有当所有条件都匹配时，组件才有资格注册。

