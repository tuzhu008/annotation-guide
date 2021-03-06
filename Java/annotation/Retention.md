# @Retention

## 定义

```
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Retention {
    /**
     * Returns the retention policy.
     * @return the retention policy
     */
    RetentionPolicy value();
}
```

## 解析

它指示带注解类型的注解将保留多长时间。

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)
@interface MyCustomAnnotation {

}
```

这里我们使用了 `RetentionPolicy.RUNTIME`。还有另外两种选择。让我们看看这是什么意思:

* `RetentionPolicy.RUNTIME`：注解应该在运行时可用，以便通过 java 反射进行检查。

* `RetentionPolicy.CLASS`：注解将在 `.class`文件中，但在运行时不可用。

* `RetentionPolicy.SOURCE`：注解将在程序的源代码中可用，既不在 `.class` 文件中，也不在运行时可用。

## RetentionPolicy

#### 定义

```java
public enum RetentionPolicy {
    /**
     * 注解将被编译器丢弃。
     */
    SOURCE,

    /**
     * 注解由编译器记录在类文件中，但不需要由 VM 在运行时保留。
     * 这是默认行为。
     */
    CLASS,

    /**
     * 注解由编译器记录在类文件中，并在运行时由 VM 保存，因此他们可以被反射性地读取。
     * 
     * @see java.lang.reflect.AnnotatedElement
     */
    RUNTIME
}
```



