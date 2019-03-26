# @Inherited

## 定义

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Inherited {
}
```

## 解析

`@Inherited` 注解表示类中使用的自定义注解应该由其所有子类继承。例如:

```java
java.lang.annotation.Inherited

@Inherited
public @interface MyCustomAnnotation {

}
```

```java
@MyCustomAnnotation
public class MyParentClass { 
  ... 
}
```

```java
public class MyChildClass extends MyParentClass { 
   ... 
}
```



