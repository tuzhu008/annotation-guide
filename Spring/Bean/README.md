# Bean

## 什么是 Bean

## Spring Bean Annotations

在 Spring 容器中配置 bean 有几种方法。我们可以使用 XML 配置声明它们。我们可以在配置类中使用 `@Bean` 注释声明bean。

或者我们可以用 `org.springframework.stereotype`  包中的任意一个注解来标记该类，其余的留给组件扫描。

### 组件扫描

如果启用了组件扫描，Spring 可以自动扫描包中的 bean。

`@ComponentScan` 配置要扫描的包，以获得带有注解 `@Configuration` 的类。我们可以使用一个 `basePackages` 或 `value` 参数\(`value` 是 `basePackages` 的别名\)直接指定基本包名:

```java
@Configuration
@ComponentScan(basePackages = "com.baeldung.annotations")
class VehicleFactoryConfig {}
```

同样，我们可以用 `basePackageClasses`  参数指向基本包中的类:

```java

```



