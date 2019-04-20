# ~~@Required\(~~Deprecated\)

## 定义

```java
@Deprecated
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Required {

}
```

## 解析

setter 方法 上的`@Required`  注解来标记我们想通过 XML 填充的依赖项:

```java
@Required
void setColor(String color) {
    this.color = color;
}
```

```java
<bean class="com.aho.annotations.Bike">
    <property name="color" value="green" />
</bean>
```

否则，将抛出 `BeanInitializationException`。

