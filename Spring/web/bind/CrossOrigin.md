# @CrossOrigin

## 定义

```java
@Target({ ElementType.METHOD, ElementType.TYPE })
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface CrossOrigin {

    /** @deprecated as of Spring 5.0, in favor of {@link CorsConfiguration#applyPermitDefaultValues} */
    @Deprecated
    String[] DEFAULT_ORIGINS = { "*" };

    /** @deprecated as of Spring 5.0, in favor of {@link CorsConfiguration#applyPermitDefaultValues} */
    @Deprecated
    String[] DEFAULT_ALLOWED_HEADERS = { "*" };

    /** @deprecated as of Spring 5.0, in favor of {@link CorsConfiguration#applyPermitDefaultValues} */
    @Deprecated
    boolean DEFAULT_ALLOW_CREDENTIALS = false;

    /** @deprecated as of Spring 5.0, in favor of {@link CorsConfiguration#applyPermitDefaultValues} */
    @Deprecated
    long DEFAULT_MAX_AGE = 1800;


    /**
     * Alias for {@link #origins}.
     */
    @AliasFor("origins")
    String[] value() default {};

    /**
     * The list of allowed origins that be specific origins, e.g.
     * {@code "https://domain1.com"}, or {@code "*"} for all origins.
     * <p>A matched origin is listed in the {@code Access-Control-Allow-Origin}
     * response header of preflight actual CORS requests.
     * <p>By default all origins are allowed.
     * <p><strong>Note:</strong> CORS checks use values from "Forwarded"
     * (<a href="https://tools.ietf.org/html/rfc7239">RFC 7239</a>),
     * "X-Forwarded-Host", "X-Forwarded-Port", and "X-Forwarded-Proto" headers,
     * if present, in order to reflect the client-originated address.
     * Consider using the {@code ForwardedHeaderFilter} in order to choose from a
     * central place whether to extract and use, or to discard such headers.
     * See the Spring Framework reference for more on this filter.
     * @see #value
     */
    @AliasFor("value")
    String[] origins() default {};

    /**
     * The list of request headers that are permitted in actual requests,
     * possibly {@code "*"}  to allow all headers.
     * <p>Allowed headers are listed in the {@code Access-Control-Allow-Headers}
     * response header of preflight requests.
     * <p>A header name is not required to be listed if it is one of:
     * {@code Cache-Control}, {@code Content-Language}, {@code Expires},
     * {@code Last-Modified}, or {@code Pragma} as per the CORS spec.
     * <p>By default all requested headers are allowed.
     */
    String[] allowedHeaders() default {};

    /**
     * The List of response headers that the user-agent will allow the client
     * to access on an actual response, other than "simple" headers, i.e.
     * {@code Cache-Control}, {@code Content-Language}, {@code Content-Type},
     * {@code Expires}, {@code Last-Modified}, or {@code Pragma},
     * <p>Exposed headers are listed in the {@code Access-Control-Expose-Headers}
     * response header of actual CORS requests.
     * <p>By default no headers are listed as exposed.
     */
    String[] exposedHeaders() default {};

    /**
     * The list of supported HTTP request methods.
     * <p>By default the supported methods are the same as the ones to which a
     * controller method is mapped.
     */
    RequestMethod[] methods() default {};

    /**
     * Whether the browser should send credentials, such as cookies along with
     * cross domain requests, to the annotated endpoint. The configured value is
     * set on the {@code Access-Control-Allow-Credentials} response header of
     * preflight requests.
     * <p><strong>NOTE:</strong> Be aware that this option establishes a high
     * level of trust with the configured domains and also increases the surface
     * attack of the web application by exposing sensitive user-specific
     * information such as cookies and CSRF tokens.
     * <p>By default this is not set in which case the
     * {@code Access-Control-Allow-Credentials} header is also not set and
     * credentials are therefore not allowed.
     */
    String allowCredentials() default "";

    /**
     * The maximum age (in seconds) of the cache duration for preflight responses.
     * <p>This property controls the value of the {@code Access-Control-Max-Age}
     * response header of preflight requests.
     * <p>Setting this to a reasonable value can reduce the number of preflight
     * request/response interactions required by the browser.
     * A negative value means <em>undefined</em>.
     * <p>By default this is set to {@code 1800} seconds (30 minutes).
     */
    long maxAge() default -1;

}
```

## 解析

`@CrossOrigin` 注解用于允许特定处理器类和/或处理器方法上的跨源请求。如果配置了适当的 HandlerMapping，则会被处理。

Spring Web MVC和Spring WebFlux都在各自的模块中通过RequestMappingHandlerMapping支持这个注释。每个类型和方法级别注释对的值都被添加到一个CorsConfiguration中，然后通过CorsConfiguration. applypermitdefaultvalues\(\)应用默认值。

组合全局和本地配置的规则通常是附加的——例如，所有全局和所有本地源。对于那些只能接受单个值的属性，如allowCredentials和maxAge，本地覆盖全局值。有关详细信息，请参见CorsConfiguration.combine\(CorsConfiguration\)。

