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



### BeanDefinition

* ROLE\_APPLICATION

* ROLE\_INFRASTRUCTURE

* ROLE\_SUPPORT









