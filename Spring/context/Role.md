# @Role

## 定义

```java
package org.springframework.context.annotation;

@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Role {

    /**
     * 为关联的 bean 设置角色提示。
     * @see BeanDefinition#ROLE_APPLICATION
     * @see BeanDefinition#ROLE_INFRASTRUCTURE
     * @see BeanDefinition#ROLE_SUPPORT
     */
    int value();

}
```

## 解析

指示给定 bean 的“角色”提示。

可用于任何直接或间接使用 `@Component` 注解的类或使用 `@Bean` 注解的方法。

如果 Component 或 Bean 定义上不存在此注解，则使用默认值 `BeanDefinition.ROLE_APPLICATION`

如果 `@Role` 出现在 `@Configuration` 类上，则表示配置类 bean 定义的角色，并且不会级联到其中定义的所有 `@Bean` 方法。例如，这种行为与 `@Lazy` 注释不同。

### BeanDefinition

* ROLE\_APPLICATION

* ROLE\_INFRASTRUCTURE

* ROLE\_SUPPORT



