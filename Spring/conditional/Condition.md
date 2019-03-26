# Condition

## 定义

```java
@FunctionalInterface
public interface Condition {

    /**
     * Determine if the condition matches.
     * @param context the condition context
     * @param metadata metadata of the {@link org.springframework.core.type.AnnotationMetadata class}
     * or {@link org.springframework.core.type.MethodMetadata method} being checked
     * @return {@code true} if the condition matches and the component can be registered,
     * or {@code false} to veto the annotated component's registration
     */
    boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata);

}
```

## 解析

要注册组件，必须匹配的单个条件。

条件将在 bean 定义即将注册之前立即进行检查，并且可以根据那时可以确定的任何标准自由否决注册。

条件必须遵循与 `BeanFactoryPostProcessor` 相同的限制，并且注意永远不要与 bean 实例交互。要对与@Configuration bean交互的条件进行更细粒度的控制，请考虑ConfigurationCondition接口。

