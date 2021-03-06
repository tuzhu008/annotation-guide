# Bean

## 什么是 Bean

在 Spring 中，由 Spring IoC 容器管理的对象叫做 beans。 bean 就是由 Spring IoC 容器实例化、组装和以其他方式管理的对象。此外 bean 只是你应用中许多对象中的一个。Beans 以及他们之间的依赖关系是通过容器配置元数据反映出来。

## Spring Bean 注解

在 Spring 容器中配置 bean 有几种方法。我们可以使用 XML 配置声明它们。我们可以在配置类中使用 `@Bean` 注释声明bean。

或者我们可以用 `org.springframework.stereotype`  包中的任意一个注解来标记该类，其余的留给组件扫描。

### 组件扫描

如果启用了组件扫描，Spring 可以自动扫描包中的 bean。

`@ComponentScan` 配置要扫描的包，以获得带有注解 `@Configuration` 的类。我们可以使用一个 `basePackages` 或 `value` 参数\(`value` 是 `basePackages` 的别名\)直接指定基本包名:

```java
@Configuration
@ComponentScan(basePackages = "com.aho.annotations")
class VehicleFactoryConfig {}
```

同样，我们可以用 `basePackageClasses`  参数指向基本包中的类:

```java
@Configuration
@ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
class VehicleFactoryConfig {}
```

两个参数都是数组，因此我们可以为每个参数提供多个包。

如果没有指定参数，则扫描发生在 `@ComponentScan` 注解类所在的包中。

`@ComponentScan` 利用了 Java 8 的重复注解特性，这意味着我们可以用它多次标记一个类:

```java
@Configuration
@ComponentScan(basePackages = "com.aho.annotations")
@ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
class VehicleFactoryConfig {
```

或者，我们可以使用 `@ComponentScan` 来指定多个 `@ComponentScan` 配置:

```java
@Configuration
@ComponentScans({ 
  @ComponentScan(basePackages = "com.aho.annotations"), 
  @ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
})
class VehicleFactoryConfig {}
```

使用 XML 配置时，配置组件扫描同样简单:

```xml
<context:component-scan base-package="com.baeldung" />
```

### 原型注释和 AOP

当我们使用 Spring 原型注解时，很容易创建一个切入点来针对所有具有特定原型的类。

例如，假设我们想要度量 DAO 层方法的执行时间。我们将利用 `@Repository` 原型创建以下方面\(使用 AspectJ 注解\):

```java
@Aspect
@Component
public class PerformanceAspect {
    @Pointcut("within(@org.springframework.stereotype.Repository *)")
    public void repositoryClassMethods() {};

    @Around("repositoryClassMethods()")
    public Object measureMethodExecutionTime(ProceedingJoinPoint joinPoint) 
      throws Throwable {
        long start = System.nanoTime();
        Object returnValue = joinPoint.proceed();
        long end = System.nanoTime();
        String methodName = joinPoint.getSignature().getName();
        System.out.println(
          "Execution of " + methodName + " took " + 
          TimeUnit.NANOSECONDS.toMillis(end - start) + " ms");
        return returnValue;
    }
}
```

在本例中，我们创建了一个切入点，它匹配使用 `@Repository` 注解的类中的所有方法。我们使用 `@Around` 通知来针对这个切入点，并确定被拦截的方法调用的执行时间

使用这种方法，我们可以向每个应用程序层添加日志记录、性能管理、审计或其他行为。

