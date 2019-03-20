## 定义

```java
package org.springframework.web.bind.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.core.annotation.AliasFor;


@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Mapping
public @interface RequestMapping {


    String name() default "";


    @AliasFor("path")
    String[] value() default {};


    @AliasFor("value")
    String[] path() default {};


    RequestMethod[] method() default {};


    String[] params() default {};


    String[] headers() default {};


    String[] consumes() default {};


    String[] produces() default {};

}
```

## 解析

* name

  指定映射的名称

* value

* path

  指定请求路径的地址，可以同时指定多个

  ```java
    // 单个路径
    @RequestMapping(value = "/hello")
    @RequestMapping(path = "/hello")

    // 多个路径
    @RequestMapping(value = { "/hello1", "/hello2" })
    @RequestMapping(path = { "/hello1", "/hello2" })
  ```

  `value` 和 `path` 是等价的，可以互换。

* method

  指定请求的方式，可以同时指定多个

  ```java
    // 单个
    @RequestMapping(value = "/hello", method = RequestMethod.POST)

    // 多个
    @RequestMapping(value = "/hello", method = { RequestMethod.POST, RequestMethod.GET)
  ```

* params

  指定参数的类型，当指定了这个参数，访问时必须带上此参数，并且值需要相同

  ```java
      
  ```

* headers

  指定 request 中必须包含某些指定的 header 值，才能让该方法处理请求

* consumes

  指定数据请求的格式（Content-Type）

  ```java
    // 单个
    @RequestMapping(value = "/getUser", consumes = "application/json")

    // 多个
    @RequestMapping(value = "/getUser", consumes = { "application/json", "application/xml")
  ```

* produces

  指定返回的内容类型（Content-Type）

  ```java
    // 单个
    @RequestMapping(value = "/getUser", produces = "application/json")

    // 多个
    @RequestMapping(value = "/getUser", produces = { "application/json", "application/xml")
  ```



