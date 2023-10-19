# Bean 管理
## `Bean` 的获取
`Bean` 由 `spring` 上下文容器管理，获取 `Bean` 需要通过 `ApplicationContext` 对象

### 根据名称获取
```java
ApplicationContext context;
/**
 \* 参数为 bean 的名称
 \* 根据 bean 的名称获取 bean，没有显式设置 bean 的名称时，通常其名称是类名首字母小写
 \* 返回值是 Object 类型，需要强制转换类型
 */
ClassName name = (ClassName) context.getBean("className");
```

### 根据类型获取
```java
ApplicationContext context;
// 根据类型获取
/**
 \* 参数为类的 class 属性
 */
ClassName name = context.getBean(ClassName.class);
```

### 根据名称和类型获取
```java
ApplicationContext context;
// 参数是 bean 的名称，和类的 class 属性
// 可用来获取上转型的 bean
ClassName name = context.getBean("className", ClassName.class);
```

## `Bean` 的作用域

### 类型

| **作用域**  | **说明**                                           |
| ----------- | -------------------------------------------------- |
| singleton   | 容器内同 名称 的 bean 只有一个实例（单例）（默认） |
| prototype   | 每次使用该 bean 时会创建新的实例（非单例）         |
| request     | 每个请求范围内会创建新的实例（web环境中，了解）    |
| session     | 每个会话范围内会创建新的实例（web环境中，了解）    |
| application | 每个应用范围内会创建新的实例（web环境中，了解）    |

### 声明作用域

使用 `@Scope` 注解声明 `Bean` 的作用域：
```java
@Scope("prototype") // ！！！
@RestController
@RequestMapping("/depts")
public class DeptController {
    // ...
}
```

## 第三方 `Bean`

如果要管理的bean对象来自于第三方（不是自定义的），是无法用 @Component 及衍生注解声明bean的，就需要用到 @Bean 注解

### @Configuration

声明配置类
```java
@Configuration
public class CommonConfig {
    // ...
}
```
### @Bean

声明 bean （与 @Configuration 联用）
```java
注解在 return 对应类型的方法上
@Configuration
public class CommonConfig {
    @Bean
    public SAXReader saxReader(){
       return new SAXReader();
   }
}
```
# tips
通过@Bean注解的name或value属性可以声明bean的名称，

如果不指定，默认bean的名称就是方法名

如果第三方bean需要依赖其它bean对象，直接在bean定义方法中设置形参即可，容器会根据类型自动装配