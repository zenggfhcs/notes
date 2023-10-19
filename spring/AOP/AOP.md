# Spring AOP

Aspect Oriented Programming（面向切面编程、面向方面编程）

基于动态代理，对特定的方法进行编程

# 导入依赖 : springboot 环境

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

# 模板编写

**`@Aspect` 注解切面，说明这是一个 aop 类（切面）**

```java
@Component
@Aspect // 说明是一个 aop 类
public class TestClass {
  // 返回值 包名.类名.方法名
  @Around("execution(* packagename.*.*(..))")
  public Object methodName(ProceedingJoinPoint point/** */) {
    /**
     * 前逻辑
     */

    // 原始方法运行
    Object ob = point.proceed();

    /**
     * 后逻辑
     */

    return ob;
  }
}
```

# 切入点类型

|     注解名     |                             说明                             |
| :------------: | :----------------------------------------------------------: |
|    @Around     | 原始方法执行**前后**执行，如果原始方法出现异常，后逻辑不会执行 |
|     @After     |                      原始方法执行**后**                      |
|    @Before     |                      原始方法执行**前**                      |
| @AfterRetuning |                    原始方法**正常返回后**                    |
| @AfterThrowing |                    原始方法**异常抛出后**                    |

# 切入点表达式

用来决定项目中的哪些方法需要加入通知

## 表达式书写

| 形式            | 作用                 |
| --------------- | -------------------- |
| @annotation(……) | 根据注解匹配         |
| execution(……)   | 根据方法的签名来匹配 |



## execution

### 格式

```java
execution([访问修饰符] 返回值 [包名.类名.]方法名(方法参数) [throws 异常])
```

其中 `[]` 部分可选，`包名.类名` 不建议省略
`包名.类名`是类的全限定名，`方法参数`中也是填参数类型的全限定名

### 使用通配符书写切入点 `*` or `..`

#### `*` 

单个独立的任意符号，可以通配任意`返回值`、`包名`、`类名`、`方法名`、任意类型的`一个参数`，也可以通配`包、类、方法名的一部分`

```java
/**
 * 第一个是任意返回值
 * 第二个是任意包名，但是只匹配一层
 * 第三个是任意类名
 * 第四个是任意方法名
 * 第五个是任意一个参数
 */
execution(* org.*.dao.*.*(*))
/**
 * 或者匹配以 abc 开头的方法
 */
execution(* org.*.dao.*.abc*(*))
```

#### `..` 

多个连续的任意符号，可以通配`任意层级的包`，或`任意类型、任意个数的参数`

```java
/**
 * 第一个是任意层包
 * 第二个是任意参数
 */
execution(* com..dao.*.*(..))
```

### 可以使用 `||`、`&&`、`!` 将多个切入点表达式连接起来

```java
// 或者 execution 会是通过判断 方法签名 跟 格式 是否匹配返回 Boolean，
// 这样对于 切入点表达式 可以当作 Boolean表达式 使用
execution(* org.*.dao.*.a*(*)) ||
execution(* org.*.dao.*.b*(*))
```

## @annotation

用于匹配标识有特定注解的方法

### 格式

```java
@annotation(注解全限定名)
```

### 例子

```java
@annotation(org.springframework.transaction.annotation.Transactional)
```

## 表达式提取

使用 `@Pointcut()` 注解 void空方法，在 `()` 内说明切入表达式

```java
@Pointcut("execution(* packagename.*.*(..))")
public void pc() {}

@Around("pc()")
public void methodName(..) {}
```

在使用的时候使用 `pc(..)` 代替 `execution(..)` 即可

如果 `pc()` 设置为private 则不能被外部引用

# 通知顺序：切点执行顺序

## 在多个切入点都匹配到目标方法，默认`依次`执行：前前后后

- 切入点类型不同的，按照切入点类型先后顺序执行

- 类型相同的，不同切面类中，按照切面类类名的字母序
  - 目标方法**前**的通知方法：字母排名靠前的**先**执行
  - 目标方法**后**的通知方法：字母排名靠前的**后**执行

## 使用 `Order` 注解自定义执行顺序

- 目标方法前的通知方法：数字小的先执行
- 目标方法后的通知方法：数字小的后执行

```java
@Order(1)
...
public class ClassName {
  ...
}
```

# 连接点

指被切入的原始方法

- 对于 `@Around` 通知，获取连接点信息只能使用  `ProceedingJoinPoint`

- 对于其他四种通知，获取连接点信息只能使用 `JoinPoint` ，它是 `ProceedingJoinPoint` 的父类型

## 获取信息

使用对应的对象，`JoinPoint` 或 `ProceedingJoinPoint`
以下使用 `point` 作为对象名

### 目标对象类名

```java
/**
 * 对象 -> 对象class -> class名
 */
String className = point.getTarget().getClass().getName();
```

### 目标方法的方法名

```java
/**
 * Signature -> 方法名
 */
String methodName = point.getSignature().getName();
```

### 目标方法运行时传入的参数

```java
/**
 * 参数可能有多个，返回值是个数组
 */
Object[] args = point.getArgs();
```

### 执行目标方法以及返回值

```java
/**
 * Object 兼容返回值类型
 */
Object res = point.proceed();
```

