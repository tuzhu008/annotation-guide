# @RequestMapping

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

`@RequestMapping` 注解用于将 web 请求映射到具有灵活方法签名的请求处理类中的方法。

Spring MVC 和 Spring WebFlux 都在各自的模块和包结构中通过 `RequestMappingHandlerMapping` 和`RequestMappingHandlerAdapter` 支持这个注解。要获得每个方法中支持的处理器方法参数和返回类型的确切列表，请使用下面的参考文档链接:

* Spring MVC [方法参数](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-ann-arguments)和[返回值](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-ann-return-types)

* Spring WebFlux [方法参数](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html#webflux-ann-arguments)和[返回值](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html#webflux-ann-return-types)

**注意**：这个注解可以在类级和方法级使用。在大多数情况下，在方法级别，应用程序更喜欢使用 HTTP 方法特定的变体`@GetMapping`、`@PostMapping`、`@PutMapping`、`@DeleteMapping` 或 `@PatchMapping` 之一。

**注意**：当使用控制器接口\(例如用于 AOP 代理\)时，确保一致地将所有映射注释\(例如 `@RequestMapping` 和 `@SessionAttributes`\)放在控制器接口上，而不是实现类上。

### 参数

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

  `RequestMethod` 枚举值：

  * GET
  * HEAD
  * POST
  * PUT
  * PATCH
  * DELETE
  * OPTIONS
  * TRACE

* params

  指定参数的类型，当指定了这个参数，访问时必须带上此参数，并且值需要相同

  ```java
  // 请求必须要带上 name 参数
  @RequestMapping(value = "/getUser", params = "name")

  // 请求不能带上 name 参数
  @RequestMapping(value = "/getUser", params != "name")

  // 请求必须要带上 name 参数，且值必须为 123
  @RequestMapping(value = "/getUser", params = "name=123")

  // 请求必须要带上 name 参数，且值不能为 123
  @RequestMapping(value = "/getUser", params = "name!=123")

  // 请求必须要带上 name 和 psw 参数，且值必须为 123 和 123456
  @RequestMapping(value = "/getUser", params = { "name=123", "psw=123456" })
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



