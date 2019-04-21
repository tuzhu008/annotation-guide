# @ResponseBody

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ResponseBody {

}
```

## 解析

`@ResponseBody`  注解表示方法返回值应该绑定到 web 响应体。支持带注注解的处理器方法。

从 4.0 版本开始，这个注释也可以添加到类型级别，在这种情况下，它是继承的，不需要添加到方法级别。



