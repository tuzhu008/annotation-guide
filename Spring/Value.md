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

我们可以注入 `engine.fuelType`  的值：

```java
@Value("${engine.fuelType}")
String fuelType;
```

## 详述

此注释可用于将值注入 spring 托管 beans 中的字段，可应用于字段或构造函数/方法参数级别。

### 设置应用程序

为了描述此注释的不同用法，我们需要配置一个简单的 Spring 应用程序配置类。

当然，我们需要一个属性文件来定义我们想要用 `@Value` 注释注入的值。因此，我们首先需要在配置类中定义一个 `@PropertySource`——使用属性文件名。

让我们定义属性文件:

```
value.from.file=Value got from the file
priority=Properties file
listOfValues=A,B,C
```

### 使用案例

作为一个基本的和几乎无用的使用例子，我们只能注入“string value”从注释字段:





