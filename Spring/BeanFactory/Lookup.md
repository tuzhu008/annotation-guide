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

`@Lookup` 注解提供了 Spring 的方法级依赖项注入支持。

### Why @Lookup

使用 `@Lookup` 注解的方法告诉 Spring 在调用方法时返回【方法返回值类型】的实例。

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

注意，在 `StudentServices` 中，我们将 `getNotification` 方法保留为存根。

这是因为 Spring 通过调用 `beanFactory.getBean(StudentNotification.class)` 覆盖了该方法，所以我们可以让它为空。

#### 注入依赖项程序

不过，更强大的是 `@Lookup` 允许我们以程序的方式注入依赖项，而这是 Provider 无法做到的。

让我们用一些状态来增强 `StudentNotification`:

```java
@Component
@Scope("prototype")
public class SchoolNotification {
    @Autowired Grader grader;

    private String name;
    private Collection<Integer> marks;

    public SchoolNotification(String name) {
        // ... set fields
    }

    // ... getters and setters

    public String addMark(Integer mark) {
        this.marks.add(mark);
        return this.grader.grade(this.marks);
    }
}
```

现在，它依赖于一些 Spring 上下文以及我们将以程序方式提供的附加上下文。

然后，我们可以向 `StudentServices` 添加一个方法，该方法获取并保存学生数据:

```java
public abstract class StudentServices {

    private Map<String, SchoolNotification> notes = new HashMap<>();

    @Lookup
    protected abstract SchoolNotification getNotification(String name);

    public String appendMark(String name, Integer mark) {
        SchoolNotification notification
          = notes.computeIfAbsent(name, exists -> getNotification(name)));
        return notification.addMark(mark);
    }
}
```

在运行时，Spring 将以同样的方式实现该方法，并使用一些额外的技巧。

首先，请注意，它可以调用一个复杂的构造函数，也可以注入其他 Spring bean，这使我们可以将 `SchoolNotification` 更像一个Spring-aware 方法。

它通过调用 `beanFactory.getBean(SchoolNotification.class, name)` 来实现 `getSchoolNotification`\)。

其次，我们有时可以使 `@ lookup` 注释的方法抽象，就像上面的例子一样。

使用抽象比使用存根好看一些，但是我们只能在不使用 _**component-scan **_或 _**@Bean**_**-管理**周围的 bean 时使用它:

```java
@Test
public void whenAbstractGetterMethodInjects_thenNewInstanceReturned() {
    // ... initialize context

    StudentServices services = context.getBean(StudentServices.class);    
    assertEquals("PASS", services.appendMark("Alex", 89));
    assertEquals("FAIL", services.appendMark("Bethany", 78));
    assertEquals("PASS", services.appendMark("Claire", 96));
}
```

通过这个设置，我们可以将 Spring 依赖项以及方法依赖项添加到 `SchoolNotification` 中。

#### 限制

尽管 `@Lookup` 功能强大，但也存在一些明显的限制:

* `@Lookup` 注解的方法\(如 `getNotification`\)在组件扫描周围类\(如 `Student`\)时必须是具体的。这是因为组件扫描跳过了抽象beans。
* 当周围的类是 `@Bean`_ _管理的时，`@Lookup`注解的方法根本不起作用。

在这些情况下，如果我们需要将原型 bean 注入到单例中，我们可以将 Provider 作为替代。

