# Annotation

Java 注释允许我们将元数据信息添加到源代码中，尽管它们不是程序本身的一部分。注解是从 JDK 5 添加到 java 的。注解对它们注解的代码的操作没有直接影响\(也就是说，它不影响程序的执行\)。

注解由 Java 提供，可以在源文件中嵌入附加信息。

```java
@interface MyAnno {
    String str();
    Int val();
}
```

`@interface`关键字用来声明一个注解。

注解里只包含方法的声明。

所有的注解类型都自动扩展 `Annocation` 接口。 `Annocation`是所有注解的超接口。

使用：

```java
@MyAnno(str = "Annotation Example", val = 100)
public static void myMethod() {
    // do something
}
```

不带形参的注解称为标记注解（marker annotation）。它的唯一作用是标记声明带有某种属性。

Java 内置了一些注解，大多是专用的，有 9 个通用的：

java.lang.annotation:

* @Ratention

* @Documented

* @Target

* @Inherited

java.lang:

* @Override

* @Deprecated

* @SafeVarargs

* @FunctionalInterface

* @SuppressWarnings

JDK 8 新增注解：

* @Repeatable

* @Native

## 注释有什么用?

1. **编译器指令**: Java中有三个内置的注释\(`@Deprecated`， `@Override` & `@SuppressWarnings`\)，可以用来给编译器提供特定的指令。例如，`@Override`注释用于指示编译器被注释的方法正在重写该方法。
2. **编译时指导**: 注解可以向编译器提供编译时指令，这些指令可以被软件构建工具进一步用于生成代码、XML 文件等。
3. **运行时指令**: 我们可以定义在运行时可用的注解，我们可以使用 java 反射访问这些注释，并可以在运行时用于向程序提供指令。

#### 我们可以在哪里使用注释?

注解可以应用于类、接口、方法和字段。例如，下面的注释应用于方法。

## 创建自定义注解

* 注解是使用 `@interface` 创建的，后面跟着注解名，如下面的示例所示。

* 注解也可以有元素。它们看起来像方法。例如在下面的代码中，我们有四个元素。我们不应该为这些元素提供实现。

* 所有注解都继承了 `java.lang.annotation.Annotation` 接口，注解不能包含任何 extends 子句。

```java
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Documented
@Target(ElementType.METHOD)
@Inherited
@Retention(RetentionPolicy.RUNTIME)
public @interface MyCustomAnnotation{
    int studentAge() default 18;
    String studentName();
    String stuAddress();
    String stuStream() default "CSE";
}
```

注意：在创建注解时设置默认值的所有元素都可以在使用注解时跳过。例如，如果我把上面的注释应用到一个类上，我会这样做：

```java
@MyCustomAnnotation(
    studentName="Chaitanya",
    stuAddress="Agra, India"
)
public class MyClass {
...
}
```

正如你所看到的，我们没有给出任何值给 `studentAge` 和 `stuStream` 元素，设置这些元素的值是可选的\(默认值已经在注解定义时设置，但是也可以像为其他元素提供值一样为有默认值的元素提供\)。但是，在使用注解时，我们**必须**提供其他元素的值\(没有设置默认值的元素\)。

**注意**：我们还可以在注解中包含数组元素。我们可以这样使用它们:

注释定义:

```java
@interface MyCustomAnnotation {
    int      count();
    String[] books();
}
```

目的:

```java
@MyCustomAnnotation(
    count=3,
    books={"C++", "Java"}
)
public class MyClass {

}
```

让我们再次回到主题:在自定义注解示例中，我们使用了以下四个注释：`@Documented` 、`@Target`、`@Inherited` & `@Retention`。让我们详细讨论一下。

