# @Repository

## 定义

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Repository {

    /**
     * The value may indicate a suggestion for a logical component name,
     * to be turned into a Spring bean in case of an autodetected component.
     * @return the suggested component name, if any (or empty String otherwise)
     */
    @AliasFor(annotation = Component.class)
    String value() default "";

}
```

## 解析

DAO 或 Repository 类通常表示应用程序中的数据库访问层，应该用 `@Repository` 注解:

```java
@Repository
class VehicleRepository {
    // ...
}
```

使用此注解的一个优点是，它启用了自动持久性异常转换。当使用 Hibernate 之类的持久性框架时，在使用 `@Repository` 注释的类中抛出的本机异常将自动转换为 Spring 的 `DataAccessExeption` 子类。

为了启用异常转换，我们需要声明我们自己的 `PersistenceExceptionTranslationPostProcessor` bean:

```java
@Bean
public PersistenceExceptionTranslationPostProcessor exceptionTranslation() {
    return new PersistenceExceptionTranslationPostProcessor();
}
```

注意，在大多数情况下，Spring 自动执行上述步骤。

或者，通过XML配置:

```xml
<bean class="org.springframework.dao.annotation.PersistenceExceptionTranslationPostProcessor"/>
```



