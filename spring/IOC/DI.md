# DI 依赖注入

容器为应用程序提供运行时，所依赖的资源，称之为依赖注入

## 扫描组件

`@SpringBootApplication` 只会扫描当前包及其子包，可以使用 `@ComponentScan` 说明需要扫描的包

```java
@ComponentScan("com.pojo")
```

## 注入冲突

当一个 Bean 存在多个实现，而在注入时又没有指定实现时，就会出现注入冲突

```java
public Interface UserService {
    // ...
}
```

```java
public class UserServiceA implements UserService {
    // ...
}
```

```java
public class UserServiceB implements UserService {
    // ...
}
```

### @Primary
当存在多个相同类型的Bean注入时，加上 `@Primary` 注解，来指定默认的实现

```java
@Primary // 为 UserService 提供默认的实现 UserServiceB
public class UserServiceB implements UserService {
    // ...
}
```

### @Qualifier

指定当前要注入的bean对象。 在 `@Qualifier` 的value属性中，指定注入的bean的名称

`@Qualifier` 注解不能单独使用，必须配合 `@Autowired` 使用

```java
// ...
public class UserController {
    // ...
    @Autowired
    @Qualifier(value="userServiceA") // 注入 userServiceA 实现
    private UserService userService;
    // ...
}
```


### @Resource

是按照bean的名称进行注入。通过name属性指定要注入的bean的名称
```java
// ...
public class UserController {
    // ...
    @Resource(name="userServiceA")
    private UserService userService;
    // ...
}
```

# tips
## @Autowired 与 @Resource

@Autowired 是spring框架提供的注解，而 @Resource 是 JDK 提供的注解

`@Autowired` 默认是按照类型注入，而 `@Resource` 默认是按照名称注入