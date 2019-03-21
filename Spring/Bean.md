# @Bean

## 定义

```java
@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Bean {

    @AliasFor("name")
    String[] value() default {};

    @AliasFor("value")
    String[] name() default {};

    @Deprecated
    Autowire autowire() default Autowire.NO;

    boolean autowireCandidate() default true;

    String initMethod() default "";

    String destroyMethod() default AbstractBeanDefinition.INFER_METHOD;

}
```

## 解析

`@Bean` 标记了一个实例化 Spring bean 的**工厂**方法:

```java
@Bean
Engine engine() {
    return new Engine();
}
```

当需要返回类型的新实例时，Spring 调用这些方法。

生成的 bean 具有与工厂方法相同的名称。如果我们想给它起个不同的名字，我们可以用这个注释的 `name` 或者 `value` 参数\(参数 `value` 是参数 `name`的别名\)：

```java
@Bean("engine")
Engine getEngine() {
    return new Engine();
}
```



