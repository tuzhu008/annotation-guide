# @ComponentScan

## 定义

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Repeatable(ComponentScans.class)
public @interface ComponentScan {

    @AliasFor("basePackages")
    String[] value() default {};

    @AliasFor("value")
    String[] basePackages() default {};

    Class<?>[] basePackageClasses() default {};

    Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;

    Class<? extends ScopeMetadataResolver> scopeResolver() default AnnotationScopeMetadataResolver.class;

    ScopedProxyMode scopedProxy() default ScopedProxyMode.DEFAULT;

    String resourcePattern() default ClassPathScanningCandidateComponentProvider.DEFAULT_RESOURCE_PATTERN;

    boolean useDefaultFilters() default true;

    Filter[] includeFilters() default {};

    Filter[] excludeFilters() default {};

    boolean lazyInit() default false;

    @Retention(RetentionPolicy.RUNTIME)
    @Target({})
    @interface Filter {

        FilterType type() default FilterType.ANNOTATION;

        @AliasFor("classes")
        Class<?>[] value() default {};

        @AliasFor("value")
        Class<?>[] classes() default {};

        String[] pattern() default {};

    }

}
```

## 解析

配置组件扫描指令，以便与 `@Configuration` 类一起使用。提供与 Spring XML 的 `<context:component-scan>` 元素并行的支持。

可以指定basepackageclass\(\)或basePackages\(\)\(或其别名值\(\)\)来定义要扫描的特定包。如果没有定义特定的包，则将对声明此注释的类的包进行扫描。

注意，&lt;context:component-scan&gt;元素有一个注解配置属性;但是，这个注释没有。这是因为在几乎所有使用@ComponentScan的情况下，默认的注释配置处理\(例如处理@Autowired和friends\)都是假定的。此外，当使用AnnotationConfigApplicationContext时，注释配置处理器总是被注册，这意味着在@ComponentScan级别禁用它们的任何尝试都将被忽略。

