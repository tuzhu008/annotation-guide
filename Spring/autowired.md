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



从4.3版本开始，我们不需要显式地用@Autowired注释构造函数，除非我们声明了至少两个构造函数。

