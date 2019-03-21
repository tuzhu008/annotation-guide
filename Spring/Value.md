# @Value

## 定义

```java
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Value {

    /**
     * The actual value expression: for example {@code #{systemProperties.myProp}}.
     */
    String value();

}
```

## 解析

我们可以使用 `@Value` 将属性值注入 bean。它与构造函数、setter和字段注入兼容。

构造函数注入:

```java
Engine(@Value("8") int cylinderCount) {
    this.cylinderCount = cylinderCount;
}
```

Setter 注入:

```java
@Autowired
void setCylinderCount(@Value("8") int cylinderCount) {
    this.cylinderCount = cylinderCount;
}
```

等价于：

```java
@Value("8")
void setCylinderCount(int cylinderCount) {
    this.cylinderCount = cylinderCount;
}
```

字段注入:

```java
@Value("8")
int cylinderCount;
```

当然，注入静态值是没有用的。因此，我们可以使用 `@Value` 中的占位符字符串来连接外部源\(例如 `.properties` 或 `.yaml` 文件\)中定义的值。

让我们假设以下 `.properties` 文件:

```java
engine.fuelType=petrol
```

我们可以注入引擎的价值。燃料类型与以下:

```java

```



