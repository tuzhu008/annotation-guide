# Summary

* [Introduction](README.md)
* [annotation](annotation.md)
* [IoC](Ioc.md)
* [AOP](AOP.md)
* [DI](DI.md)
* [properties](properties.md)

## Java

* [@Target](/Java/Target.md)
* [@Retention](/Java/Retention.md)
* [@Inherited](/Java/Inherited.md)
* [@Override](/Java/Override.md)
* [@Deprecated](/Java/Deprecated.md)
* [@SuppressWarnings](/Java/SuppressWarnings.md)
* [@Documented](/Java/Documented.md)
* [@Repeatable](/Java/Repeatable.md)
* [@Native](/Java/Native.md)
  
## Spring

* [Bean](/Spring/Bean/README.md)
  * [@Component](/Spring/Bean/Component.md)
  * [@Repository](/Spring/Bean/Repository.md)
  * [@Service](/Spring/Bean/Service.md)
  * [@Controller](/Spring/Bean/Controller.md)
  * [@Configuration](/Spring/Bean/Configuration.md)
  
* [依赖注入相关](Spring/requestmapping/di.md)
  * [@Autowired](/Spring/autowired.md)
  * [@Bean](/Spring/Bean.md)
  * [@Qualifier](/Spring/Qualifier.md)
  * [@Required](/Spring/Required.md)
  * [@Value](/Spring/Value.md)
  * [@DependsOn](/Spring/DependsOn.md)
  * [@Lazy](/Spring/Lazy.md)
  * [@Lookup](/Spring/Lookup.md)
  * [@Primary](/Spring/Primary.md)
  * [@Scope](/Spring/Scope.md)
  
* [上下文配置相关](Spring/requestmapping/READMA.md)
  * [@Profile](/Spring/Profile.md)
  * [@Import](/Spring/Import.md)
  * [@ImportResource](/Spring/ImportResource.md)
  * [@PropertySource](/Spring/PropertySource.md)
  * [@PropertySources](/Spring/PropertySources.md)
  
* [条件注册](Spring/conditional/READEME.md)
  * [@Conditional](Spring/conditional/Conditional.md)
  * [Condition](Spring/conditional/Condition.md)

* Cache
  * [@EnableCaching]()
  * [@Cacheable]()
  * [@CachePut]()
  * [@CacheEvict]()
  * [@Cacheable]()

Web
  * [@ControllerAdvice]()
  * [@RestControllerAdvice]()
  * [@ExceptionHandler]()

* validation
  * [Validated]()

## Spring Boot

* [@SpringBootApplication](/SpringBoot/springBootApplication.md)
* [@EnableAutoConfiguration](/SpringBoot/EnableAutoConfiguration.md)
* [@SpringBootConfiguration](/SpringBoot/SpringBootConfiguration.md)
* [@ConfigurationProperties](/SpringBoot/ConfigurationProperties.md)
* [@EnableConfigurationProperties](/SpringBoot/EnableConfigurationProperties.md)

* [条件注册](SpringBoot/conditional/README.md)
  * [@ConditionalOnClass](SpringBoot/conditional/ConditionalOnClass.md)
  * [@ConditionalOnBean](SpringBoot/conditional/ConditionalOnBean.md)
  * [@ConditionalOnCloudPlatform](SpringBoot/conditional/ConditionalOnCloudPlatform.md)
  * [@ConditionalOnEnabledResourceChain](SpringBoot/conditional/ConditionalOnEnabledResourceChain.md)
  * [@ConditionalOnExpression](SpringBoot/conditional/ConditionalOnExpression.md)
  * [@ConditionalOnJava](SpringBoot/conditional/ConditionalOnJava.md)
  * [@ConditionalOnJndi](SpringBoot/conditional/ConditionalOnJndi.md)
  * [@ConditionalOnProperty](SpringBoot/conditional/ConditionalOnProperty.md)
  * [@ConditionalOnResource](SpringBoot/conditional/ConditionalOnResource.md)
  * [@ConditionalOnSingleCandidate](SpringBoot/conditional/ConditionalOnSingleCandidate.md)
  * [@ConditionalOnWebApplication](SpringBoot/conditional/ConditionalOnWebApplication.md)
  * [@ConditionalOnNotWebApplication](SpringBoot/conditional/ConditionalOnNotWebApplication.md)
  * [@ConditionalOnRepositoryType](SpringBoot/conditional/ConditionalOnRepositoryType.md)
  * [@ConditionalOnMissingClass](SpringBoot/conditional/ConditionalOnMissingClass.md)
  * [@ConditionalOnMissingBean](SpringBoot/conditional/ConditionalOnMissingBean.md)
  * [@ConditionalOnMissingFilterBean](SpringBoot/conditional/ConditionalOnMissingFilterBean.md)
  
* [@RequestMapping](/Spring/requestmapping/requestmapping.md)
  * [@GetMapping](/Spring/requestmapping/getmapping.md)
  * [@PostMapping](/Spring/requestmapping/postMapping.md)
  * [@PutMapping](/Spring/requestmapping/putMapping.md)
  * [@DeleteMapping](/Spring/requestmapping/deleteMapping.md)
  * [@PatchMapping](/Spring/requestmapping/patchMapping.md)
* [@Spring](/Spring/spring.md)
* [@SpringBootApplication](/Spring/SpringBootApplication.md)
* [@RestController](/Spring/RestController.md)
* [@ResponseBody](/Spring/ResponseBody.md)
* [@ComponentScan](/Spring/ComponentScan.md)
* [@Resource](/Spring/Resource.md)
* [@RequestParam](/Spring/RequestParam.md)
* [@PathVariable]()
* [@CookieValue]()
* [@RequestHeader]()
* [@ModelAttribute]()
* [@SessionAttributes]()
* [@InitBinder]()
* [@AutoConfigureAfter]()

* [@Inject](/Spring/Inject.md)
* [@Conditional](/Spring/Conditional.md)

* [@Service]()
  
* [@Endpoint]()
* [@ReadOperation]()
* [@WriteOperation]()
* [@DeleteOperation]()
* [@ConditionalOnEnabledEndpoint]()

Task
  * [@Scheduled]()
  * [@Async]()
  * [@EnableScheduling]()
  * [@EnableScheduling]()

* [@EnableAdminServer]()

* WebSocket
  * [@EnableWebSocket]()
  * [@ServerEndpoint]()
  * [@OnOpen]()
  * [@OnMessage]()
  * [@OnClose]()
  * [@OnError]()

* Test
  * [@SpringBootTest]()
  
## javax

* persistence
  * [@Id](Spring/MyBatis/Delete.md)
  * [@GeneratedValue](Spring/MyBatis/Delete.md)
  * [@Table](Spring/MyBatis/Delete.md)
* validation
  * [@NotNull]()
  * [@NotBlank]()
  * [@DecimalMin]()
  * [@Pattern]()
  * [@Constraint]()
  
## hibernate

* validator
  * [@Length]()


## aspectj

* [@Aspect]()
* [@Around]()

## Shiro

* [@RequiresRoles]()

## MyBatis

* [@Mapper](Spring/MyBatis/Delete.md)
* [@MapperScan](Spring/MyBatis/Delete.md)
* [@Insert](Spring/MyBatis/Delete.md)
* [@Delete](Spring/MyBatis/Delete.md)
* [@Select](Spring/MyBatis/Delete.md)
* [@Update](Spring/MyBatis/Delete.md)
* [@Param](Spring/MyBatis/Delete.md)
* [@Results](Spring/MyBatis/Delete.md)
* [@Result](Spring/MyBatis/Delete.md)

## 其他
  * Elasticsearch
  * Lombok
  * Junit
    * [@RunWith]()
    * [@Test]()

## swagger

* [@Api]()
* [@ApiIgnore]()
* [@ApiOperation]()
* [@ApiParam]()
* [@ApiModel]()
* [@ApiProperty]()
* [@ApiImplicitParam]()
* [@ApiImplicitParams]()
* [@ApiResponse]()
* [@ApiResponses]()
* [@ApiError]()
* [@ApiModelProperty]()
  
## Message Queue

  * Kafka
  * Activemq
  * Zeromq
  * Rabbitmq
    * [@RabbitListener]()