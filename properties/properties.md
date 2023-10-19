# properties

## 属性注入

### @Value

单属性注入，属性注解

```java
@Value("${com.study.name}")
```

### @ConfigurationProperties

多属性注入，类注解

```java
@ConfiguratinProperties(prefix="com.study")
```

其中 prefix 属性指定需要注入的属性的统一前缀，并且类的属性名要与被注入的属性名一致？

依赖于

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
</dependency>
```

