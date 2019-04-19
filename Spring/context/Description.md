# @Description

## 定义

```java
package org.springframework.context.annotation;

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Description {

    /**
     * The textual description to associate with the bean definition.
     */
    String value();

}
```

## 解析

向派生的 bean 定义添加文本描述

```java
@Configuration
public class AppConfig {

    @Bean
    @Description("Provides a basic example of a bean")
    public Foo foo() {
        return new Foo();
    }

}
```



