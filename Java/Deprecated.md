# @Deprecated

## 定义

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})
public @interface Deprecated {
}
```

## 解析

`@Deprecated` 注解用于通知编译器特定的方法、类或字段已被弃用，当有人试图使用它们时，它应该生成一个警告。

### “Deprecated” 是什么意思?

废弃的类或方法就是这样。它不再重要。它是如此的不重要，事实上，你不应该再使用它，因为它已经被取代，并可能在未来停止存在。

Java 提供了一种表达弃用的方法，因为随着类的发展，它的 API\(应用程序编程接口\)不可避免地会发生变化：为保持一致性而重命名方法，添加新的更好的方法，以及更改字段。但这些变化带来了一个问题。您需要保留旧 API，直到开发人员转换到新API为止，但是您不希望他们继续对旧API进行编程，在这种情况下，您可以使用内置注释禁用特定的项。

