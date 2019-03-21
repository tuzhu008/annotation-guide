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

```java
@Value("string value")
private String stringValue;
```

使用 `@PropertySource` 注解允许我们使用 `@Value` 注解处理来自属性文件的值。在下面的例子中，我们得到“Value get from the file”分配给字段:

```java
@Value("${value.from.file}")
private String valueFromFile;
```

我们还可以使用相同的语法从系统属性设置值。假设我们已经定义了一个名为 `systemValue` 的系统属性，并查看下面的示例:

```java
@Value("${systemValue}")
private String systemValue;
```

可以为可能没有定义的属性提供默认值。在这个例子中，值“some default”将被注入:

```java
@Value("${unknown.param:some default}")
private String someDefault;
```

如果在属性文件中将相同的属性定义为系统属性，那么将应用系统属性。

假设我们有一个属性优先级，它定义为一个值为“system property”的系统属性，并定义为属性文件中的其他内容。在下面的代码中，值将是“System property”:

```java
@Value("${priority}")
private String prioritySystemProperty;
```

有时候我们需要注入一些值。将它们定义为属性文件中单个属性或系统属性的逗号分隔值，并将它们注入到数组中，这样会很方便。在第一部分中，我们在属性文件的列表值中定义了逗号分隔的值，因此在下面的例子中数组值将是\[" A "， " B "， " C "\]:

```java
@Value("${listOfValues}")
private String[] valuesArray;
```

## 使用SpEL的高级示例

我们还可以使用 SpEL 表达式来获取值。如果我们有一个名为 priority 的系统属性，那么它的值将在下一个例子中应用到该字段:

```java
@Value("#{systemProperties['priority']}")
private String spelValue;
```

如果我们没有定义系统属性，那么将分配 `null` 值。为了防止这种情况，我们可以在 SpEL 表达式中提供一个默认值。在下面的例子中，如果系统属性没有定义，我们会得到“一些默认”值:

```java
@Value("#{systemProperties['unknown'] ?: 'some default'}")
private String spelSomeDefault;
```

此外，我们还可以使用来自其他 bean 的字段值。假设我们有一个名为 `someBean` 的 bean，其字段 `somvaluue`等于 `10`。然后在这个例子中将 `10` 分配给字段:

```java
@Value("#{someBean.someValue}")
private Integer someBeanValue;
```

### 对映射使用 @Value

我们还可以使用 `@Value` 注解注入 Map 属性。

首先，我们需要在属性文件中的 `{key: ' value '}`表单中定义属性:

```java
valuesMap={key1: '1', key2: '2', key3: '3'}
```

**注意，映射中的值必须用单引号括起来。**

现在我们可以从属性文件中以映射的形式注入这个值:

```java
@Value("#{${valuesMap}}")
private Map<String, Integer> valuesMap;
```

如果我们需要在 Map中 获取特定键的值，我们所要做的就是在表达式中添加键的名称:

```java
@Value("#{${valuesMap}.key1}")
private Integer valuesMapKey1;
```

如果我们不确定映射是否包含某个键，我们应该选择一个更安全的表达式，它不会抛出异常，但在没有找到键时将值设置为 `null`:

```java
@Value("#{${valuesMap}['unknownKey']}")
private Integer unknownMapKey;
```

我们还可以为可能不存在的属性或键设置默认值:

```java
@Value("#{${unknownMap : {key1: '1', key2: '2'}}}")
private Map<String, Integer> unknownMap;

@Value("#{${valuesMap}['unknownKey'] ?: 5}")
private Integer unknownMapKeyWithDefaultValue;
```

映射条目也可以在注入之前过滤。假设我们只需要得到那些值大于 1 的项:

```java
@Value("#{${valuesMap}.?[value>'1']}")
private Map<String, Integer> valuesMapFiltered;
```

我们还可以使用 `@Value` 注解注入所有当前系统属性:



