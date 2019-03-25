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
3. **运行时指令**: 我们可以定义在运行时可用的注解，我们可以使用java反射访问这些注释，并可以在运行时用于向程序提供指令。我们将在这篇文章的后面通过一个例子来讨论这个问题。



