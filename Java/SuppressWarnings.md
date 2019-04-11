# @SuppressWarnings

## 定义

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
    String[] value();
}
```

## 解析

这个注解指示编译器忽略特定的警告。例如，在下面的代码中，我调用了一个弃用的方法\(假设方法 `deprecatedMethod()` 被标记为`@Deprecated` 注释\)，因此编译器应该生成一个警告，但是我使用了 `@SuppressWarnings` 注解，该注释将抑制该警告。

```java
@SuppressWarnings("deprecation")
void myMethod() {
    myObject.deprecatedMethod();
}
```



