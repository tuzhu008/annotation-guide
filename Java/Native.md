# @Native

## 定义

```
@Documented
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.SOURCE)
public @interface Native {
}
```

## 解析

指示可以从本机代码引用定义常数值的字段。生成本机头文件的工具可以使用注解作为提示，以确定是否需要头文件，如果需要，还应包含哪些声明。

