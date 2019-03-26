# @Repeatable

## 定义

```
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Repeatable {
    /**
     * Indicates the <em>containing annotation type</em> for the
     * repeatable annotation type.
     * @return the containing annotation type
     */
    Class<? extends Annotation> value();
}
```

## 解析

该注解用于指示声明它\(元\)注释的注释类型是Repeatable。@Repeatable的值表示可重复注释类型的包含注释类型。

