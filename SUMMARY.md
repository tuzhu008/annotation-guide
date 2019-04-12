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

* data
  * [@ReadOnlyProperty](Spring/data/ReadOnlyProperty.md)
  * [@Version](Spring/data/Version.md)
  * [@PersistenceConstructor](Spring/data/PersistenceConstructor.md)
  * [@CreatedDate](Spring/data/CreatedDate.md)
  * [@CreatedBy](Spring/data/CreatedBy.md)
  * [@LastModifiedDate](Spring/data/LastModifiedDate.md)
  * [@Id](Spring/data/Id.md)
  * [@TypeAlias](Spring/data/TypeAlias.md)
  * [@Transient](Spring/data/Transient.md)
  * [@AccessType](Spring/data/AccessType.md)
  * [@Reference](Spring/data/Reference.md)
  * [@Persistent](Spring/data/Persistent.md)
  * [@LastModifiedBy](Spring/data/LastModifiedBy.md)
  * [@QueryAnnotation](Spring/data/QueryAnnotation.md)

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
  * [@Constraint](Javax/validation/Constraint.md)
  * [@GroupSequence](Javax/validation/GroupSequence.md)
  * [@OverridesAttribute](Javax/validation/OverridesAttribute.md)
  * [@ReportAsSingleViolation](Javax/validation/ReportAsSingleViolation.md)
  * [@Valid](Javax/validation/Valid.md)
  * constraints
    * [@AssertFalse](Javax/validation/constraints/AssertFalse.md)
    * [@AssertTrue](Javax/validation/constraints/AssertTrue.md)
    * [@DecimalMax](Javax/validation/constraints/DecimalMax.md)
    * [@DecimalMin](Javax/validation/constraints/DecimalMin.md)
    * [@Digits](Javax/validation/constraints/Digits.md)
    * [@Email](Javax/validation/constraints/Email.md)
    * [@Future](Javax/validation/constraints/Future.md)
    * [@FutureOrPresent](Javax/validation/constraints/FutureOrPresent.md)
    * [@Max](Javax/validation/constraints/Max.md)
    * [@Min](Javax/validation/constraints/Min.md)
    * [@Negative](Javax/validation/constraints/Negative.md)
    * [@NegativeOrZero](Javax/validation/constraints/NegativeOrZero.md)
    * [@NotBlank](Javax/validation/constraints/NotBlank.md)
    * [@NotEmpty](Javax/validation/constraints/NotEmpty.md)
    * [@NotNull](Javax/validation/constraints/NotNull.md)
    * [@Null](Javax/validation/constraints/Null.md)
    * [@Past](Javax/validation/constraints/Past.md)
    * [@PastOrPresent](Javax/validation/constraints/PastOrPresent.md)
    * [@Pattern](Javax/validation/constraints/Pattern.md)
    * [@Positive](Javax/validation/constraints/Positive.md)
    * [@PositiveOrZero](Javax/validation/constraints/PositiveOrZero.md)
    * [@Size](Javax/validation/constraints/Size.md)

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
  * [@AdviceName](Others/aspectj/AdviceName.md)
  * [@After](Others/aspectj/After.md)
  * [@AfterReturning](Others/aspectj/AfterReturning.md)
  * [@AfterThrowing](Others/aspectj/AfterThrowing.md)
  * [@Around](Others/aspectj/Around.md)
  * [@Aspect](Others/aspectj/Aspect.md)
  * [@Before](Others/aspectj/Before.md)
  * [@DeclareAnnotation](Others/aspectj/DeclareAnnotation.md)
  * [@DeclareError](Others/aspectj/DeclareError.md)
  * [@DeclareMixin](Others/aspectj/DeclareMixin.md)
  * [@DeclareParents](Others/aspectj/DeclareParents.md)
  * [@DeclarePrecedence](Others/aspectj/DeclarePrecedence.md)
  * [@DeclareWarning](Others/aspectj/DeclareWarning.md)
  * [@Pointcut](Others/aspectj/Pointcut.md)
  * [@RequiredTypes](Others/aspectj/RequiredTypes.md)
  * [@SuppressAjWarnings](Others/aspectj/SuppressAjWarnings.md)

* Shiro
  * [@RequiresAuthentication](Others/shiro/RequiresAuthentication.md)
  * [@RequiresGuest](Others/shiro/RequiresGuest.md)
  * [@RequiresPermissions](Others/shiro/RequiresPermissions.md)
  * [@RequiresRoles](Others/shiro/RequiresRoles.md)
  * [@RequiresUser](Others/shiro/RequiresUser.md)

* MyBatis
  * [@Arg](Others/Mybatis/Arg.md)
  * [@AutomapConstructor](Others/Mybatis/AutomapConstructor.md)
  * [@CacheNamespace](Others/Mybatis/CacheNamespace.md)
  * [@CacheNamespaceRef](Others/Mybatis/CacheNamespaceRef.md)
  * [@Case](Others/Mybatis/Case.md)
  * [@ConstructorArgs](Others/Mybatis/ConstructorArgs.md)
  * [@Delete](Others/Mybatis/Delete.md)
  * [@DeleteProvider](Others/Mybatis/DeleteProvider.md)
  * [@Flush](Others/Mybatis/Flush.md)
  * [@Insert](Others/Mybatis/Insert.md)
  * [@InsertProvider](Others/Mybatis/InsertProvider.md)
  * [@Lang](Others/Mybatis/Lang.md)
  * [@Many](Others/Mybatis/Many.md)
  * [@MapKey](Others/Mybatis/MapKey.md)
  * [@Mapper](Others/Mybatis/Mapper.md)
  * [@One](Others/Mybatis/One.md)
  * [@Options](Others/Mybatis/Options.md)
  * [@Param](Others/Mybatis/Param.md)
  * [@Property](Others/Mybatis/Property.md)
  * [@Result](Others/Mybatis/Result.md)
  * [@ResultMap](Others/Mybatis/ResultMap.md)
  * [@Results](Others/Mybatis/Results.md)
  * [@ResultType](Others/Mybatis/ResultType.md)
  * [@Select](Others/Mybatis/Select.md)
  * [@SelectKey](Others/Mybatis/SelectKey.md)
  * [@SelectProvider](Others/Mybatis/SelectProvider.md)
  * [@TypeDiscriminator](Others/Mybatis/TypeDiscriminator.md)
  * [@Update](Others/Mybatis/Update.md)
  * [@UpdateProvider](Others/Mybatis/UpdateProvider.md)
  

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
  
