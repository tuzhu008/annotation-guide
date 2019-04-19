# @ComponentScans

## 定义

```java
package org.springframework.context.annotation;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
public @interface ComponentScans {

    ComponentScan[] value();

}
```

## 解析

一次声明多个 [`@ComponentScan`](/Spring/context/ComponentScan.md)

