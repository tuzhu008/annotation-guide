## _@EnableAutoConfiguration_ {#enable-autoconfiguration}

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Import(ImportAutoConfigurationImportSelector.class)
public @interface ImportAutoConfiguration {

    /**
     * The auto-configuration classes that should be imported. This is an alias for
     * {@link #classes()}.
     * @return the classes to import
     */
    @AliasFor("classes")
    Class<?>[] value() default {};

    /**
     * The auto-configuration classes that should be imported. When empty, the classes are
     * specified using an entry in {@code META-INF/spring.factories} where the key is the
     * fully-qualified name of the annotated class.
     * @return the classes to import
     */
    @AliasFor("value")
    Class<?>[] classes() default {};

    /**
     * Exclude specific auto-configuration classes such that they will never be applied.
     * @return the classes to exclude
     */
    Class<?>[] exclude() default {};

}
```

## 解析

启用 Spring 应用程序上下文的自动配置，尝试猜测和配置您可能需要的 bean。自动配置类通常基于类路径和定义的 bean 来应用。例如，如果类路径上有 tomcat-embedded.jar，则可能需要一个 `TomcatServletWebServerFactory`\(除非定义了自己的`ServletWebServerFactory` bean\)。

当使用 `SpringBootApplication` 时，上下文的自动配置是自动启用的，因此添加这个注释没有额外的效果。

自动配置试图尽可能地智能化，当您定义更多自己的配置时，它就会后退。您总是可以手动 `exclude()` 您永远不想应用的任何配置\(如果您没有访问这些配置的权限，则使用 `excludeName()`\)。您还可以通过 \``spring.autoconfigure.exclude`\` 排除它们。在注册了用户定义的 bean 之后，总是会应用自动配置。

使用 `@EnableAutoConfiguration` 注释的类包\(通常通过 `@SpringBootApplication`\)具有特定的意义，通常用作“缺省值”。例如，它将用于扫描 `@Entity` 类。通常建议将 `@EnableAutoConfiguration`\(如果不使用 `@SpringBootApplication)`放在根包中，以便可以搜索所有子包和类。

自动配置类是常规的 Spring 配置 bean。它们是使用 `SpringFactoriesLoader` 机制定位的\(针对该类进行键控\)。通常，自动配置bean 是  `@Conditional` bean\(通常使用 `@ConditionalOnClass` 和@ConditionalOnMissingBean注释\)。

