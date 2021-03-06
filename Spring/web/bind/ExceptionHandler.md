# @ExceptionHandler

## 定义

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ExceptionHandler {

    /**
     * Exceptions handled by the annotated method. If empty, will default to any
     * exceptions listed in the method argument list.
     */
    Class<? extends Throwable>[] value() default {};

}
```

## 解析

`@ExceptionHandler` 用于处理特定处理器类和/或处理器方法中的异常。

使用此注解的处理器方法允许具有非常灵活的签名。它们可以有以下类型的参数，按任意顺序排列:

* 异常参数：声明为一般异常或更具体的异常。如果注解本身没有通过 `value()` 缩小异常类型的范围，那么这也可以作为映射提示。

* Request 和/或 Response 对象\(通常来自Servlet API\)。您可以选择任何特定的请求/响应类型，例如 `ServletRequest` / `HttpServletRequest`。

* Session 对象：通常是 HttpSession。这种类型的参数将强制出现相应的会话。因此，这样的参数永远不会为空。注意，会话访问可能不是线程安全的，特别是在 Servlet 环境中：如果允许多个请求同时访问会话，请考虑将 `synchronizeOnSession` 标志切换为 `true`。

* WebRequest 或 NativeWebRequest。允许通用请求参数访问以及请求/会话属性访问，而不需要与本机 Servlet API 绑定。

* [Local](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html?is-external=true)，当前请求区域的 Local\(由可用的最特定的区域设置解析器决定，即 Servlet 环境中配置的 LocaleResolver\)。

* InputStream / Reader，用于访问请求内容。这将是 Servlet API 公开的原始 InputStream/Reader。

* OutputStream / Writer， 用于生成响应内容 。这将是 Servlet API 公开的原始 OutputStream/Writer。

* Model，作为从处理程序方法返回模型映射的替代方法。注意，所提供的模型没有预先填充常规模型属性，因此总是空的，以便为特定于异常的视图准备模型。

处理器方法支持以下返回类型:

* 一个 ModelAndView 对象\(来自 Servlet MVC\)。

* 一个 Model 对象，视图名通过 `RequestToViewNameTranslator` 隐式地确定。

* 一个 Map 对象，用于公开模型，视图名称通过 `RequestToViewNameTranslator` 隐式确定。

* 一个 View 对象。

* 一个 String 值，它被解释为视图名。

* `@ResponseBody` 注解的方法\(只有 Servlet\)，用来设置响应内容。返回值将使用[消息转换器](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/converter/HttpMessageConverter.html)转换为响应流。

* 一个 `HttpEntity <?>`或 `ResponseEntity<?>` 对象\(仅限 Servlet\)，用来设置响应头和内容。`ResponseEntity` 主体将使用消息转换器转换并写入响应流。

* void，如果方法本身处理响应\(通过直接编写响应内容，声明 ServletResponse / HttpServletResponse 类型的参数\)，或者视图名应该通过 RequestToViewNameTranslator 隐式确定\(而不是在处理程序方法签名中声明响应参数\)，则为 void。

对于特定的 HTTP 错误状态，可以将 `@ExceptionHandler` 注解与 `@ResponseStatus` 结合使用

