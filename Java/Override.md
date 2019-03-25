# @Override

## 定义

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```

## 解析

`@Override` 标记表示符号覆盖父类中具有相同名称的符号。

此标记是为了与闭包编译器兼容而提供的。默认情况下，JSDoc 自动标识覆盖父类的符号。

如果 JSDoc 注释包含 `@inheritdoc` 标记，则不需要包含 `@Override` 标记。`@inheritdoc` 标记的存在意味着@override标记的存在。

