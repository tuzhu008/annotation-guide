# @Lookup

## 定义

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Lookup {

    /**
     * This annotation attribute may suggest a target bean name to look up.
     * If not specified, the target bean will be resolved based on the
     * annotated method's return type declaration.
     */
    String value() default "";

}
```

## 解析

使用 `@Lookup` 注解的方法告诉 Spring 在调用方法时返回【方法返回值类型】的实例。

## 详述

`@Lookup` 注释了解 Spring 的方法级依赖项注入支持。

### Why @Lookup

使用 `@Lookup` 注释的方法告诉 Spring 在调用方法时返回【方法返回值类型】的实例。

本质上，Spring 将覆盖我们的带注解的方法，并使用方法的返回类型和参数作为 `BeanFactory#getBean` 的参数。

`@Lookup` 用于:

* 将原型作用域 bean 注入到单例 bean 中\(类似于提供者\)
* 注入依赖项程序

还要注意，`@Lookup` 是 XML 元素 `lookup-method` 的 Java 等效项。

### 使用 @Lookup

#### 将原型作用域 bean 注入到一个单例 bean 中

如果我们碰巧决定拥有一个原型 Spring bean，那么我们几乎马上就会面临一个问题:我们的单例 Spring bean 如何访问这些原型Spring bean ?

现在，Provider 当然是一种方法，尽管 `@Lookup` 在某些方面更加通用。

首先，让我们创建一个原型 bean，稍后我们将把它注入到一个单例 bean中:

```java
@Component
@Scope("prototype")
public class SchoolNotification {
    // ... prototype-scoped state
}
```

如果我们创建一个使用 `@Lookup` 的单例 bean:

```java
@Component
public class StudentServices {

    // ... member variables, etc.

    @Lookup
    public SchoolNotification getNotification() {
        return null;
    }

    // ... getters and setters
}
```

使用 `@Lookup`，我们可以通过我们的单例 bean 获得一个通知实例:

```java
@Test
public void whenLookupMethodCalled_thenNewInstanceReturned() {
    // ... initialize context
    StudentServices first = this.context.getBean(StudentServices.class);
    StudentServices second = this.context.getBean(StudentServices.class);
        
    assertEquals(first, second); 
    assertNotEquals(first.getNotification(), second.getNotification()); 
}

```



