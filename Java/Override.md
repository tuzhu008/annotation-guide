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

在上面的例子中，我们在子类中覆盖了方法 `displaymethod()`。即使我们不使用 `@Override` 注释，程序仍然可以正常运行，不会出现任何问题，您可能想知道为什么要使用这个注释。让我们讨论一下:

