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

#### **Maven 配置文件**

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

```
mvn clean package -Pprod
```

这个命令将打包用于 prod 配置文件的应用程序。它还在应用程序运行时为该应用程序应用 `spring.profiles.active` 值 \`_prod_\`

#### _**@ActiveProfile **_**in Tests**

测试可以很容易地指定哪些配置文件是活动的——使用 `@ActiveProfile` 注解来启用特定的配置文件:

```java
@ActiveProfiles("dev")
```

总之，我们研究了激活配置文件的多种方法。现在让我们看看哪个优先级高于另一个，如果你使用多个优先级会发生什么——从最高优先级到最低优先级:

1. Context parameter in _web.xml_
2. _WebApplicationInitializer_
3. JVM System parameter
4. Environment variabl
5. Maven profile

### 默认的配置文件

任何没有指定配置文件的 bean 都属于 “default” 配置件。

Spring 还提供了一种在没有其他配置文件处于活动状态时设置默认配置文件的方法——使用 `spring.profiles.default` 属性。

### 获取激活的配置文件

一旦配置文件被激活，我们可以在运行时检索活动的配置文件，只需注入 `Environment`:

```java
public class ProfileManager {
    @Autowired
    Environment environment;

    public void getActiveProfiles() {
        for (final String profileName : environment.getActiveProfiles()) {
            System.out.println("Currently active profile - " + profileName);
        }   
    }
}
```

## 配置文件使用示例

考虑这样一个场景:我们必须为开发和生产环境维护数据源配置。让我们创建一个公共接口 `DatasourceConfig`，它需要由两个数据源实现来实现:

```java
public interface DatasourceConfig {
    public void setup();
}
```



