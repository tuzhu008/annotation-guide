# @Override

## 定义

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```

## 解析

`@Override` 注解用于在子类中重写方法。通常，新手开发人员会忽略这个特性，因为在覆盖方法时并不强制使用这个注释。在这里，我们将讨论为什么应该使用 `@Override` 注释，以及为什么它被认为是java编码中的最佳实践。

让我们先举个例子来理解它是如何使用的，然后我们会详细讨论:

### 示例

下面的例子演示了如何指示一个方法覆盖其父类中的一个方法:

方法重写父类：

```java
/**
 * @classdesc Abstract class representing a network connection.
 * @class
 */
function Connection() {}

/**
 * Open the connection.
 */
Connection.prototype.open = function() {
    // ...
};


/**
 * @classdesc Class representing a socket connection.
 * @class
 * @augments Connection
 */
function Socket() {}

/**
 * Open the socket.
 * @override
 */
Socket.prototype.open = function() {
    // ...
};
```



