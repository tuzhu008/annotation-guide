# Summary

* [Introduction](README.md)
* [annotation](annotation.md)
* [IoC](Ioc.md)
* [AOP](AOP.md)
* [DI](DI.md)
* [properties](properties.md)

## Java

* [@SuppressWarnings](/Java/SuppressWarnings.md)
* [@Deprecated](/Java/Deprecated.md)
* [@FunctionalInterface](/Java/FunctionalInterface.md)
* [@Override](/Java/Override.md)
* [@SafeVarargs](/Java/SafeVarargs.md)

* annotation
  * [@Target](/Java/annotation/Target.md)
  * [@Retention](/Java/annotation/Retention.md)
  * [@Inherited](/Java/annotation/Inherited.md)
  * [@Documented](/Java/annotation/Documented.md)
  * [@Repeatable](/Java/annotation/Repeatable.md)
  * [@Native](/Java/annotation/Native.md)
  
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

* Web
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

* Task
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

## Message Queue

  * Kafka
  * Activemq
  * Zeromq
  * Rabbitmq
    * [@RabbitListener]()
  
## 其他

* hibernate
  * validator
    * [@Length]()

* aspectj
  * [@AdviceName](Spring/aspectj/AdviceName.md)
  * [@After](Spring/aspectj/After.md)
  * [@AfterReturning](Spring/aspectj/AfterReturning.md)
  * [@AfterThrowing](Spring/aspectj/AfterThrowing.md)
  * [@Around](Spring/aspectj/Around.md)
  * [@Aspect](Spring/aspectj/Aspect.md)
  * [@Before](Spring/aspectj/Before.md)
  * [@DeclareAnnotation](Spring/aspectj/DeclareAnnotation.md)
  * [@DeclareError](Spring/aspectj/DeclareError.md)
  * [@DeclareMixin](Spring/aspectj/DeclareMixin.md)
  * [@DeclareParents](Spring/aspectj/DeclareParents.md)
  * [@DeclarePrecedence](Spring/aspectj/DeclarePrecedence.md)
  * [@DeclareWarning](Spring/aspectj/DeclareWarning.md)
  * [@Pointcut](Spring/aspectj/Pointcut.md)
  * [@RequiredTypes](Spring/aspectj/RequiredTypes.md)
  * [@SuppressAjWarnings](Spring/aspectj/SuppressAjWarnings.md)

* Shiro
  * [@RequiresRoles]()

* MyBatis
  * [@Arg](Spring/Mybatis/Arg.md)
  * [@AutomapConstructor](Spring/Mybatis/AutomapConstructor.md)
  * [@CacheNamespace](Spring/Mybatis/CacheNamespace.md)
  * [@CacheNamespaceRef](Spring/Mybatis/CacheNamespaceRef.md)
  * [@Case](Spring/Mybatis/Case.md)
  * [@ConstructorArgs](Spring/Mybatis/ConstructorArgs.md)
  * [@Delete](Spring/Mybatis/Delete.md)
  * [@DeleteProvider](Spring/Mybatis/DeleteProvider.md)
  * [@Flush](Spring/Mybatis/Flush.md)
  * [@Insert](Spring/Mybatis/Insert.md)
  * [@InsertProvider](Spring/Mybatis/InsertProvider.md)
  * [@Lang](Spring/Mybatis/Lang.md)
  * [@Many](Spring/Mybatis/Many.md)
  * [@MapKey](Spring/Mybatis/MapKey.md)
  * [@Mapper](Spring/Mybatis/Mapper.md)
  * [@One](Spring/Mybatis/One.md)
  * [@Options](Spring/Mybatis/Options.md)
  * [@Param](Spring/Mybatis/Param.md)
  * [@Property](Spring/Mybatis/Property.md)
  * [@Result](Spring/Mybatis/Result.md)
  * [@ResultMap](Spring/Mybatis/ResultMap.md)
  * [@Results](Spring/Mybatis/Results.md)
  * [@ResultType](Spring/Mybatis/ResultType.md)
  * [@Select](Spring/Mybatis/Select.md)
  * [@SelectKey](Spring/Mybatis/SelectKey.md)
  * [@SelectProvider](Spring/Mybatis/SelectProvider.md)
  * [@TypeDiscriminator](Spring/Mybatis/TypeDiscriminator.md)
  * [@Update](Spring/Mybatis/Update.md)
  * [@UpdateProvider](Spring/Mybatis/UpdateProvider.md)
  

* Elasticsearch

* Lombok

* Junit
  * [@RunWith]()
  * [@Test]()

* swagger
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
  
