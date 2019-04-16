## @SpringBootConfiguration {#enable-autoconfiguration}

## 定义

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
}
```

## 解析

`@SpringBootConfiguration` 继承自 `@Configuration`，二者功能也一致，标注当前类是配置类，

并会将当前类内声明的一个或多个以 `@Bean` 注解标记的方法的实例纳入到spring容器中，并且实例名就是方法名。





