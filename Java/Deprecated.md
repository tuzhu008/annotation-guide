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

Java 提供了一种表达弃用的方法，因为随着类的发展，它的 API\(应用程序编程接口\)不可避免地会发生变化：为保持一致性而重命名方法，添加新的更好的方法，以及更改字段。但这些变化带来了一个问题。您需要保留旧 API，直到开发人员转换到新 API 为止，但是您不希望他们继续对旧 API 进行编程，在这种情况下，您可以使用内置注释禁用特定的项。

### 如何弃用?

我们使用 `@Deprecated` 注解来弃用方法、类或字段，并在注释部分使用 `@Deprecated` Javadoc 标记来通知开发人员弃用的原因，以及可以用什么来替代这个弃用的方法、类或字段。举个例子:

```java
class DeprecatedDemo {
   /* @deprecated This field is replaced by 
    * MAX_NUM field
    */
   @Deprecated
   int num=10;

   final int MAX_NUM=10;

   /* @deprecated As of release 1.5, replaced 
    * by myMsg2(String msg, String msg2)
    */
   @Deprecated
   public void myMsg(){
       System.out.println("This method is marked as deprecated");
   }

   public void myMsg2(String msg, String msg2){
       System.out.println(msg+msg2);
   }

   public static void main(String a[]){      
        DeprecatedDemo obj = new DeprecatedDemo();
        obj.myMsg();
        System.out.println(obj.num);
   }
}
```

输出：

```
This method is marked as deprecated
10
```

在上面的例子中，我们有一个废弃的方法和一个废弃的字段。正如您所看到的，我们已经使用 `@Deprecated`注解对它们进行了标记，并且在注解部分中，我们使用了 `@Deprecated`  javadoc 标记\(用于文档目的\)来通知程序员使用什么来替代它。

注意：当您使用弃用类型\(方法、类或字段\)编译器不会抛出一个编译错误它只会显示一个警告，让你知道这是不可能有一个更好的选择，你可以找到在评论部分通过寻找 `@deprecated` 标签。

