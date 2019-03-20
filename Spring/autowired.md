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
	```

* 字段注入:

	```java
	```




