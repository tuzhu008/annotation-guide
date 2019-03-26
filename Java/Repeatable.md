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

该注解用于指示声明它\(元\)注解的注解类型是 _repeatable_。`@Repeatable` 的值表示可重复注解类型的包含注释类型。

