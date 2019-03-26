# @SpringBootConfiguration

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {

}
```

## 解析

`@SpringBootConfiguration` 继承自 `@Configuration` 二者功能也一致，标注当前类是配置类，并会将当前类内声明的一个或多个以 `@Bean` 注解标记的方法的实例纳入到 spring 容器中，并且实例名就是方法名。

`@SpringBootConfiguration`可以作为 Spring 的标准 `@Configuration` 注解的替代，以便自动找到配置\(例如在测试中\)。

```java
@SpringBootConfiguration
public class Config {
    @Bean
    public Map createMap(){
        Map map = new HashMap();
        map.put("username","gxz");
        map.put("age",27);
        return map;
    }
}
```

应用程序应该只包含一个 `@SpringBootConfiguration`，大多数惯用的  Spring Boot 应用程序将从`@ SpringBootApplication` 继承它。

```
/*
 * 发现@SpringBootApplication是一个复合注解，
 * 包括@ComponentScan，和@SpringBootConfiguration，@EnableAutoConfiguration
 * 
 */

@RestController
@SpringBootApplication
public class App 
{   
    
    @RequestMapping(value="/hello")
    public String Hello(){
        return "hello";
    }
    
    
    @Bean
    public Runnable createRunnable() {
        return () -> System.out.println("spring boot is running");
    }

    public static void main( String[] args )
    {
        System.out.println( "Hello World!" );
        ConfigurableApplicationContext context = SpringApplication.run(App.class, args);
        context.getBean(Runnable.class).run();
        System.out.println(context.getBean(User.class));
        Map map = (Map) context.getBean("createMap");   //注意这里直接获取到这个方法bean
        int age = (int) map.get("age");
        System.out.println("age=="+age);
    }
    
    
    @Bean
    public EmbeddedServletContainerFactory servletFactory(){
        TomcatEmbeddedServletContainerFactory tomcatFactory = 
                new TomcatEmbeddedServletContainerFactory();
        //tomcatFactory.setPort(8011);
        tomcatFactory.setSessionTimeout(10,TimeUnit.SECONDS);
        return tomcatFactory;
        
    }
}
```



