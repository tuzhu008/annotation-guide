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

如果 JSDoc 注释包含 `@inheritdoc` 标记，则不需要包含 `@Override` 标记。`@inheritdoc` 标记的存在意味着 `@Override`标记的存在。

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



