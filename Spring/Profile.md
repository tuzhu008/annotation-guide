# @Profile

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional(ProfileCondition.class)
public @interface Profile {

    /**
     * The set of profiles for which the annotated component should be registered.
     */
    String[] value();

}
```

## 解析

如果我们希望 Spring 仅在特定配置文件处于活动状态时才使用 `@Component` 类或 `@Bean` 方法，我们可以用 `@Profile` 标记它。我们可以使用注释的 `value` 参数来配置配置文件的名称:

```java
@Component
@Profile("sportDay")
class Bike implements Vehicle {}
```

## 详述

Profiles 是框架的一个核心特性—它允许我们将 bean 映射到不同的 profiles—例如，dev、test、prod。

然后，我们可以在不同的环境中激活不同的配置文件来引导我们需要的 bean

### 在 Bean 上使用 _**@Profile**_

让我们从简单的开始，看看如何使 bean 属于特定的 _**Profile**_。使用 `@Profile` 注解——我们将 bean 映射到那个特定的概要文件;注解只接受一个\(或多个\)概要文件的名称。

考虑一个基本场景——我们有一个 bean，它应该只在开发期间活动，而不是部署在生产环境中。我们用一个 “dev” 配置文件来注解这个 bean，它只会在开发过程中出现在容器中——在生产中，dev 不会被激活:

```java
@Component
@Profile("dev")
public class DevDatasourceConfig
```

作为一种快速的旁注，配置文件名称也可以用 `NOT` 操作符作为前缀，例如 \`!dev\` 将它们从配置文件中排除。

在下面的例子中，只有当 “dev” 配置文件不活动时，组件才会被激活:

```java
@Component
@Profile("!dev")
public class DevDatasourceConfig
```

### 用 XML 声明配置文件

配置文件也可以用 XML 配置- `<beans>` 标签具有 `profiles` 属性，该属性接受用逗号分隔的适用配置文件的值:

```xml
<beans profile="dev">
    <bean id="devDatasourceConfig" class="org.baeldung.profiles.DevDatasourceConfig" />
</beans>
```

### 设置配置文件

下一步是激活和设置陪自己文件，以便在容器中注册相应的 bean。

这可以通过多种方式实现——我们将在下面几节中对此进行探讨。

#### 通过 WebApplicationInitializer 接口

在 web 应用程序中，可以使用 `WebApplicationInitializer` 以编程方式配置 `ServletContext`。

这可以非常方便地来设置我们的活动配置文件:

```java
@Configuration
public class MyWebApplicationInitializer 
  implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {

        servletContext.setInitParameter(
          "spring.profiles.active", "dev");
    }
}
```

#### 通过 _**ConfigurableEnvironment**_

您还可以直接在 environment 上设置配置文件:

```java
@Autowired
private ConfigurableEnvironment env;
...
env.setActiveProfiles("someProfile");
```

### _**web.xml 中的 **_**Context 参数**

同样，可以在 web 应用程序的 web.xml 中使用 `context-param` 激活配置文件:

```xml
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/app-config.xml</param-value>
</context-param>
<context-param>
    <param-name>spring.profiles.active</param-name>
    <param-value>dev</param-value>
</context-param>
```

### **JVM 系统参数**

配置文件名称也可以通过 JVM 系统参数传入。作为参数传递的配置文件名称将在应用程序启动时激活:

```java
-Dspring.profiles.active=dev
```

#### 环境变量

在 Unix 环境中，还可以通过环境变量激活配置文件:

```
export spring_profiles_active=dev
```

### **Maven 配置文件** {#maven-profile}

还可以通过指定 Maven 配置文件中的 `spring.profiles.active` 配置属性激活 Spring 配置文件。

在每个 Maven 配置文件中，我们都可以设置 `spring.profiles.active`属性:

```xml
<profiles>
    <profile>
        <id>dev</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <properties>
            <spring.profiles.active>dev</spring.profiles.active>
        </properties>
    </profile>
    <profile>
        <id>prod</id>
        <properties>
            <spring.profiles.active>prod</spring.profiles.active>
        </properties>
    </profile>
</profiles>
```

它的值将用于替换 _application.properties 中的 _`@spring.profiles.active@` 中的占位符:

```
spring.profiles.active=@spring.profiles.active@
```

现在，我们需要启用 pom.xm l中的资源过滤:

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>true</filtering>
        </resource>
    </resources>
    ...
</build>
```

并添加一个 `-P` 参数来切换将应用哪个 Maven 配置文件:

