# @Target

## 定义

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Target {
    /**
     * Returns an array of the kinds of elements an annotation type
     * can be applied to.
     * @return an array of the kinds of elements an annotation type
     * can be applied to
     */
    ElementType[] value();
}
```

## 解析

它指定了我们可以在哪里使用注解。例如: 在下面的代码中，我们将目标类型定义为 `METHOD`，这意味着下面的注释只能用于方法。

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target({ElementType.METHOD})
public @interface MyCustomAnnotation {

}
```

```java
public class MyClass {
   @MyCustomAnnotation
   public void myMethod()
   {
       //Doing something
   }
}
```

注意：

1. 如果你不定义任何 Target 类型，这意味着注解可以应用于任何元素。
2. 除了 `ElementType.METHOD`，注解可以具有以下可能的 Target 值。

   * ElementType.METHOD

   * ElementType.PACKAGE

   * ElementType.PARAMETER

   * ElementType.TYPE

   * ElementType.ANNOTATION\_TYPE

   * ElementType.CONSTRUCTOR

   * ElementType.LOCAL\_VARIABLE

   * ElementType.FIELD

单个 ElementType 常量在 `@Target` 注解中出现多次是编译时错误。例如，下面的 `@Target` 元注释是非法的:

