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



