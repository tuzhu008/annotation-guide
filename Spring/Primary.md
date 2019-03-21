# @Primary

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Primary {

}
```

## 解析

有时我们需要定义同一类型的多个 bean。在这些情况下，注入将不成功，因为 Spring 不知道我们需要哪个 bean。

我们已经看到了处理此场景的选项：使用 `@Qualifier` 标记所有连接点，并指定所需 bean 的名称。

然而，大多数时候我们需要一个特定的 bean，很少需要其他 bean。我们可以用 `@Primary` 来简化这种情况：如果我们用 `@Primary` 来标记最常用的 bean，它将会被选择在不合格的注入点:

```java
@Component
@Primary
class Car implements Vehicle {}

@Component
class Bike implements Vehicle {}

@Component
class Driver {
    @Autowired
    Vehicle vehicle;
}

@Component
class Biker {
    @Autowired
    @Qualifier("bike")
    Vehicle vehicle;
}
```

在前面的示例中，`Car` 是主要车辆。因此，在 `Driver` 类中，Spring 注入了一个 `Car` bean。当然，在Biker bean中，field vehicle的值将是一个Bike对象，因为它是限定的。

