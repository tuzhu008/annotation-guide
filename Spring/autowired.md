# @AutoWired

## 定义

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

## 解析

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

从 4.3 版本开始，我们不需要显式地用 `@Autowired` 注释构造函数，除非我们声明了至少两个构造函数。

从Spring 2.5开始，该框架引入了一种由@Autowired注解驱动的新型依赖注入。这个注释允许Spring解析并将协作bean注入到bean中。

在本教程中，我们将了解如何启用自动装配、连接bean的各种方法、使bean可选、使用@Qualifier注释解决bean冲突以及潜在的异常场景。

启用 _@Autowired 注解_

如果您在应用程序中使用基于 Java 的配置，您可以通过使用 AnnotationConfigApplicationContext 加载 spring 配置来启用注释驱动的注入，如下所示:

```java
@Configuration
@ComponentScan("com.aho.autowire.sample")
public class AppConfig {}
```

作为一种替代方法，在 Spring XML 中，可以通过在 Spring XML 文件中像这样声明它: `<context:annotation-config/>` 来启用它。

使用 _@Autowired_

一旦启用了注解注入，就可以在属性、setter 和构造函数上使用自动装配。

@Autowired on Properties

注释可以直接用于属性，因此不需要 getter 和 setter:

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

### _**@Autowired **_**on Setters**

@Autowired 注解可以用于 setter 方法。在下面的例子中，当在 setter 方法上使用注释时，在创建 `FooService` 时，使用`FooFormatter`实例调用 `setter` 方法:

```java
public class FooService {

    private FooFormatter fooFormatter;

    @Autowired
    public void setFooFormatter(FooFormatter fooFormatter) {
            this.fooFormatter = fooFormatter;
    }
}
```

### _**@Autowired **_**on Constructors**

@Autowired注解也可以用在构造函数上。在下面的示例中，当在构造函数上使用注释时，在创建FooService时，将FooFormatter的一个实例作为参数注入构造函数:

```java
public class FooService {

    private FooFormatter fooFormatter;

    @Autowired
    public FooService(FooFormatter fooFormatter) {
        this.fooFormatter = fooFormatter;
    }
}
```

## @Autowired and Optional Dependencies {#dependencies}

> Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException:  
> No qualifying bean of type \[com.autowire.sample.FooDAO\] found for dependency:  
> expected at least 1 bean which qualifies as autowire candidate for this dependency.  
> Dependency annotations:  
> {@org.springframework.beans.factory.annotation.Autowired\(required=true\)}

```java
public class FooService {

    @Autowired(required = false)
    private FooDAO dataAccessor; 

}
```

### **Autowiring by**_**@Qualifier**_

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



