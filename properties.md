# 配置文件

```java
// 指定配置文件的名称，默认为 application（application.properties）
// 在初期需要根据spring.config.name和spring.config.location决定加载哪个文件
// 所以它们必须定义为 environment 属性（通常为OS env，系统属性或命令行参数）。
spring.config.name = override

// 引用一个明确的路径（目录位置或文件路径列表以逗号分割
// 如果spring.config.location包含目录（相对于文件），那它们应该以 / 结尾
spring.config.location = classpath:/override.properties
```



