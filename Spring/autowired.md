# @AutoWired

## 1. 定义

```java
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Autowired {

    /**
     * Declares whether the annotated dependency is required.
     * <p>Defaults to {@code true}.
     */
    boolean required() default true;

}
```

## 2. 解析

我们可以使用 `@Autowired` 标记 Spring 将要解析和注入的依赖项。我们可以使用这个注解进行构造函数、setter 或字段的注入

* 构造函数注入

  ```java
  class Car {
      Engine engine;

      @Autowired
      Car(Engine engine) {
          this.engine = engine;
      }
  }
  ```

* Setter 注入

  ```java
  class Car {
      Engine engine;

      @Autowired
      void setEngine(Engine engine) {
          this.engine = engine;
      }
  }
  ```

* 字段注入:

  ```java
  class Car {
      @Autowired
      Engine engine;
  }
  ```

`@Autowired` 有一个布尔参数 `required`，默认值为 `true`。当 Spring 找不到合适的 bean 连接时，它会调整 Spring 的行为。如果为真，则抛出异常，否则将不连接任何内容。

注意，如果我们使用构造函数注入，所有构造函数参数都是强制性的。

## 3. 详述

从 4.3 版本开始，我们不需要显式地用 `@Autowired` 注释构造函数，除非我们声明了至少两个构造函数。

从 Spring 2.5 开始，该框架引入了一种由 `@Autowired` 注解驱动的新型依赖注入。这个注释允许 Spring 解析并将协 bean 注入到bean中。

### 3.1 启用 _@Autowired 注解_

如果您在应用程序中使用基于 Java 的配置，您可以通过使用 AnnotationConfigApplicationContext 加载 spring 配置来启用注释驱动的注入，如下所示:

```java
@Configuration
@ComponentScan("com.aho.autowire.sample")
public class AppConfig {}
```

作为一种替代方法，在 Spring XML 中，可以通过在 Spring XML 文件中像这样声明它: `<context:annotation-config/>` 来启用它。

### 3.2 使用 _@Autowired_

一旦启用了注解注入，就可以在属性、setter 和构造函数上使用自动装配。

#### 3.2.1 @Autowired on Properties

注解可以直接用于属性，因此不需要 getter 和 setter:

```java
@Component("fooFormatter")
public class FooFormatter {

    public String format() {
        return "foo";
    }
}
```

```java
@Component
public class FooService {
    @Autowired
    private FooFormatter fooFormatter;
}
```

在上面的示例中，创建 `FooService` 时，Spring 查找并注入 fooFormatter。

#### 3.2.2 _**@Autowired **_**on Setters**

`@Autowired` 注解可以用于 setter 方法。在下面的例子中，当在 setter 方法上使用注释时，在创建 `FooService` 时，使用`FooFormatter`实例调用 `setter` 方法:

```java
public class FooService {

    private FooFormatter fooFormatter;

    @Autowired
    public void setFooFormatter(FooFormatter fooFormatter) {
            this.fooFormatter = fooFormatter;
    }
}
```

#### 3.2.3 _**@Autowired **_**on Constructors**

`@Autowired` 注解也可以用在构造函数上。在下面的示例中，当在构造函数上使用注释时，在创建 `FooService` 时，将`FooFormatter` 的一个实例作为参数注入构造函数:

```java
public class FooService {

    private FooFormatter fooFormatter;

    @Autowired
    public FooService(FooFormatter fooFormatter) {
        this.fooFormatter = fooFormatter;
    }
}
```

## 3.3 @Autowired 和可选的依赖关系 {#dependencies}

Spring 期望在构造依赖 bean 时 `@Autowired` 依赖项可用。如果框架无法解析 bean 进行连接，它将抛出下面引用的异常，并阻止Spring 容器成功启动:

> Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException:  
> No qualifying bean of type \[com.autowire.sample.FooDAO\] found for dependency:  
> expected at least 1 bean which qualifies as autowire candidate for this dependency.  
> Dependency annotations:  
> {@org.springframework.beans.factory.annotation.Autowired\(required=true\)}

为了避免这种情况发生，可以将 bean 指定为可选的，如下所示:

```java
public class FooService {

    @Autowired(required = false)
    private FooDAO dataAccessor; 

}
```

### 3.4 Autowire 消除歧义

默认情况下，Spring 根据类型\( Car、FooFormatter\)解析 `@Autowired` 条目。如果容器中有多个相同类型的 bean 可用，框架将抛出一个致命异常，表明有多个 bean 可用来进行自动装配。

#### 3.4.1 **Autowiring by **_**@Qualifier**_

`@Qualifier` 注解可以用来提示和缩小所需的 bean:

```java
@Component("fooFormatter")
public class FooFormatter implements Formatter {

    public String format() {
        return "foo";
    }
}
```

```java
@Component("barFormatter")
public class BarFormatter implements Formatter {

    public String format() {
        return "bar";
    }
}
```

```java
public class FooService {

    @Autowired    
    private Formatter formatter;
}
```

因为 Spring 容器可以注入两种 Formatter 的具体实现，所以在构造 FooService 时，Spring 会抛出一个名 `NoUniqueBeanDefinitionException` 异常:

> \| `Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException:No qualifying bean of type [com.autowire.sample.Formatter] is defined:expected single matching bean but found2: barFormatter,fooFormatter` \|  
> \| :--- \|

这可以通过使用 `@Qualifier` 注解缩小实现来避免:

```java
public class FooService {

    @Autowired
    @Qualifier("fooFormatter")
    private Formatter formatter;
```

当 Spring 发现多个相同类型的 bean 时，通过使用特定实现的名称指定 `@Qualifier`\(在本例中为 `fooFormatter`\)，可以避免歧义。

请注意，`@Qualifier` 注释的值与我们的 `FooFormatter` 实现的 `@Component` 注释中声明的名称匹配。

### **Autowiring by Custom Qualifier**

Spring 允许我们创建自己的@Qualifier注解。要创建自定义限定符，请定义一个注解并在定义中提供@Qualifier注解，如下所示:

```java
@Qualifier
@Target({
  ElementType.FIELD, ElementType.METHOD, ElementType.TYPE, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
public @interface FormatterType {

    String value();

}
```

一旦定义了FormatterType，就可以在各种实现中使用它来指定自定义值:

```java
@FormatterType("Foo")
@Component
public class FooFormatter implements Formatter {

    public String format() {
        return "foo";
    }
}
```

```java
@FormatterType("Bar")
@Component
public class BarFormatter implements Formatter {

    public String format() {
        return "bar";
    }
}
```

一旦实现被注解，自定义限定符注解可以使用如下:

```java
@Component
public class FooService {

    @Autowired
    @FormatterType("Foo")
    private Formatter formatter;

}
```

@Target 注解中指定的值限制了限定符用于标记注入点的位置。

在上面的代码片段中，限定符可以用来消除Spring可以将bean注入字段、方法、类型和参数的歧义。

### **Autowiring by Name**

作为回退，Spring 使用bean名称作为默认限定符值。

因此，通过定义bean属性名\(在本例中为fooFormatter\)， Spring将其与fooFormatter实现匹配，并在构建FooService时注入特定的实现:

```java
public class FooService {

    @Autowired
    private Formatter fooFormatter;

}
```

虽然@Qualifier和bean名称回退匹配都可以用于缩小到特定bean的范围，但自动装配实际上是按类型注入的，这是使用容器特性的最佳方式。

