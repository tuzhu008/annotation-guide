# 配置文件

```java
// 指定配置文件的名称，默认为 application（application.properties）
// 在初期需要根据spring.config.name和spring.config.location决定加载哪个文件
// 所以它们必须定义为 environment 属性（通常为OS env，系统属性或命令行参数）。
spring.config.name = override

// 引用一个明确的路径（目录位置或文件路径列表以逗号分割
// 如果spring.config.location包含目录（相对于文件），那它们应该以 / 结尾
spring.config.location = classpath:/override.properties

// 激活的配置文件 application-test.properties
spring.profiles.active=test
```

### server

 server 下的配置定义在 ServerProperties.class 中

```
public class ServerProperties {
    private Integer port;
    private InetAddress address;
    @NestedConfigurationProperty
    private final ErrorProperties error = new ErrorProperties();
    private Boolean useForwardHeaders;
    private String serverHeader;
    private DataSize maxHttpHeaderSize = DataSize.ofKilobytes(8L);
    private Duration connectionTimeout;
    @NestedConfigurationProperty
    private Ssl ssl;
    @NestedConfigurationProperty
    private final Compression compression = new Compression();
    @NestedConfigurationProperty
    private final Http2 http2 = new Http2();
    private final ServerProperties.Servlet servlet = new ServerProperties.Servlet();
    private final ServerProperties.Tomcat tomcat = new ServerProperties.Tomcat();
    private final ServerProperties.Jetty jetty = new ServerProperties.Jetty();
    private final ServerProperties.Undertow undertow = new ServerProperties.Undertow();

    // 其他内容
}
```

## log

```
logging.level.root = WARN
logging.level.org.springframework.web = DEBUG
logging.level.org.hibernate = ERROR

#比如 mybatis sql日志
logging.level.org.mybatis = INFO
logging.level.mapper所在的包 = DEBUG

# 将日志写入到指定的文件中，默认为相对路径，可以设置成绝对路径
logging.file = test.log

# 将名为 spring.log 写入到指定的文件夹中，如（/var/log）
logging.path = /logs

# 定义输出到控制台的格式（不支持JDK Logger）
logging.pattern.console=%d{yyyy/MM/dd-HH:mm:ss} [%thread] %-5level %logger- %msg%n

# 定义输出到文件的格式（不支持JDK Logger）
logging.pattern.file=%d{yyyy/MM/dd-HH:mm} [%thread] %-5level %logger- %msg%n

# 限制日志文件大小
logging.file.max-size
# 限制日志保留天数
logging.file.max-history

# 配置自定义日志文件
logging.config
```

## datasource

```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&allowMultiQueries=true&useSSL=false
spring.datasource.username=root
spring.datasource.password=123456
```

## mybatis

```

```



