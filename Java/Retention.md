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

* `RetentionPolicy.RUNTIME`：注释应该在运行时可用，以便通过java反射进行检查。

* `RetentionPolicy.CLASS`：注释将在. CLASS文件中，但在运行时不可用。

* RetentionPolicy。源代码：注释将在程序的源代码中可用，既不在.class文件中，也不在运行时可用。



以上就是本主题“Java注释”的全部内容。如果你有任何问题，请在下面留言。

