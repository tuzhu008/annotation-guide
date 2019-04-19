# @Description

## 定义

```java
package org.springframework.context.annotation;

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Description {

    /**
     * 与 bean 定义关联的文本描述。
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



