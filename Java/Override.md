# @Override

## 定义

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```

## 解析

`@Override` 注解用于在子类中重写方法。通常，新手开发人员会忽略这个特性，因为在覆盖方法时并不强制使用这个注释。在这里，我们将讨论为什么应该使用 `@Override` 注释，以及为什么它被认为是 java 编码中的最佳实践。

让我们先举个例子来理解它是如何使用的，然后我们会详细讨论:

### 示例

下面的例子演示了如何指示一个方法覆盖其父类中的一个方法:

```java
class ParentClass {
    public void displayMethod(String msg){
        System.out.println(msg);
    }
}

class SubClass extends ParentClass {
    @Override
    public void displayMethod(String msg){
        System.out.println("Message is: "+ msg);
    }
    public static void main(String args[]){
        SubClass obj = new SubClass();
        obj.displayMethod("Hey!!");
    }
}
```

在上面的例子中，我们在子类中覆盖了方法 `displaymethod()`。即使我们不使用注解 `@Override` ，程序仍然可以正常运行，不会出现任何问题，您可能想知道为什么要使用这个注解。让我们讨论一下:

### 为什么使用 @Override 注解

重写方法时使用 `@Override` 注解被认为是 java 编码的最佳实践，因为它有以下两个优点:

1. 如果程序员在重写过程中犯了任何错误，例如错误的方法名、错误的参数类型，那么您将得到编译时错误。通过使用这个注解，您可以指示编译器重写这个方法。如果不使用注解，则子类方法将作为子类中的新方法\(而不是覆盖方法\)。
2. 它提高了代码的可读性。因此，如果更改覆盖方法的签名，那么所有覆盖特定方法的子类都会抛出编译错误，这最终将帮助您更改子类中的签名。如果您的应用程序中有很多类，那么当您更改方法的签名时，这个注释将真正帮助您识别需要更改的类。



