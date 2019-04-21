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

* Session对象:通常是HttpSession。这种类型的参数将强制出现相应的会话。因此，这样的参数永远不会为空。注意，会话访问可能不是线程安全的，特别是在Servlet环境中:如果允许多个请求同时访问会话，请考虑将“synchronizeOnSession”标志切换为“true”。

* WebRequest或NativeWebRequest。允许通用请求参数访问以及请求/会话属性访问，而不需要与本机Servlet API绑定。

* 当前请求区域设置的区域设置\(由可用的最特定的区域设置解析器决定，即Servlet环境中配置的LocaleResolver\)。

* 用于访问请求内容的InputStream / Reader。这将是Servlet API公开的原始InputStream/Reader。

* 用于生成响应内容的OutputStream / Writer。这将是Servlet API公开的原始OutputStream/Writer。

* 作为从处理程序方法返回模型映射的替代方法。注意，所提供的模型没有预先填充常规模型属性，因此总是空的，以便为特定于异常的视图准备模型。

处理程序方法支持以下返回类型:

* 一个ModelAndView对象\(来自Servlet MVC\)。

* 一个模型对象，视图名通过RequestToViewNameTranslator隐式地确定。

* 用于公开模型的映射对象，视图名称通过RequestToViewNameTranslator隐式确定。

* 一个视图对象。

* 一个字符串值，它被解释为视图名。

* @ResponseBody带注释的方法\(只有servlet\)来设置响应内容。返回值将使用消息转换器转换为响应流。

* 一个HttpEntity &lt; ?&gt;或ResponseEntity &lt; ?对象\(仅限servlet\)来设置响应头和内容。ResponseEntity主体将使用消息转换器转换并写入响应流。

* 如果方法本身处理响应\(通过直接编写响应内容，声明ServletResponse / HttpServletResponse类型的参数\)，或者视图名应该通过RequestToViewNameTranslator隐式确定\(而不是在处理程序方法签名中声明响应参数\)，则为void。

对于特定的 HTTP 错误状态，可以将ExceptionHandler注释与@ResponseStatus结合使用。

