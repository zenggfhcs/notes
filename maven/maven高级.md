# Maven

# 模块化

方便管理维护、资源共享

# 继承与聚合

## 继承

继承描述的是两个工程间的关系，与`Java`中的继承相似，子工程可以继承父工程中的配置信息，常见于依赖关系的继承

用于简化依赖配置、统一管理依赖

### 实现

父工程的打包方式设置为 pom，便于打包

```xml
<groupId>...</groupId>
<artifactId>...</artifactId>
<version>...</version>
<packaging>pom</packaging>
```

子工程中配置继承关系，在 `parent.relativePath` 标签中配置父工程 `pom.xml` 的路径，如果没有配置，将从本地仓库/远程仓库查找该工程

```xml
<parent>
	<groupId>...</groupId>
	<artifactId>...</artifactId>
	<version>...</version>
    <relativePath>../xxx-parent/pom.xml</relativePath>
</parent>
```

在父工程中配置子工程共有依赖，统一管理，子工程会继承父工程的依赖，若父子工程都配置了同一个依赖的不同版本，以子工程的为准。

## 聚合

聚合项目中所依赖的所有模块，将多个模块组织成一个整体，简化项目构建

### 聚合工程

一个不具有业务功能的“空”工程（有且仅有一个 `pom.xml` ），用于快速构建项目

一般来说，继承关系中的父工程与聚合关系中的聚合工程是同一个

### 实现

在maven中，我们可以在聚合工程中通过 `<moudules>` 设置当前聚合工程所包含的子模块的名称

```xml
<!--聚合其他模块-->
<modules>
    <module>../xxx-utils</module>
</modules>
```

### 使用

在聚合工程上使用项目的一键构建（一键清理clean、一键编译compile、一键测试test、一键打包package、一键安装install等）

# 版本锁定

在maven中，可以在父工程的 `pom.xml` 中通过 `<dependencyManagement>` 来统一管理依赖版本

父工程：

```xml
<!--统一管理依赖版本-->
<dependencyManagement>
    <dependencies>
        <!--JWT令牌-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

子工程：

```xml
<dependencies>
    <!--JWT令牌-->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
        <!-- 无需配置版本号 -->
    </dependency>
</dependencies>
```

子工程引入依赖时，无需指定 `<version>` 版本号，父工程统一管理。变更依赖版本，只需在父工程中统一变更。

## 属性配置

通过自定义属性及属性引用的形式，在父工程中将依赖的版本号进行集中管理维护

自定义属性：

```xml
<properties>
	<lombok.version>1.18.24</lombok.version>
</properties>
```

引用属性：

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>${lombok.version}</version>
</dependency>
```

---

**`<dependencyManagement>` 与 `<dependencies>` 的区别是什么?**

- `<dependencies>` 是直接依赖，在父工程配置了依赖，子工程会直接继承下来。 
- `<dependencyManagement>` 是统一管理依赖版本，不会直接依赖，还需要在子工程中引入所需依赖(无需指定版本)

# 私服

企业内部共享模块实现

## 使用流程

**1. 设置私服的访问用户名/密码（在自己maven安装目录下的conf/settings.xml中的servers中配置）**

```xml
<!-- release -->
<server>
    <id>maven-releases</id>
    <username>admin</username>
    <password>admin</password>
</server>
<!-- snapshot -->
<server>
    <id>maven-snapshots</id>
    <username>admin</username>
    <password>admin</password>
</server>
```

**2. 设置私服依赖下载的仓库组地址（在自己maven安装目录下的conf/settings.xml中的mirrors、profiles中配置）**

mirrors：

```xml
<mirror>
    <id>maven-public</id>
    <mirrorOf>*</mirrorOf>
    <!-- 私服 url -->
    <url>http://192.168.150.101:8081/repository/maven-public/</url>
</mirror>
```
profiles：
```xml
<profile>
    <id>allow-snapshots</id>
        <activation>
        	<activeByDefault>true</activeByDefault>
        </activation>
    <repositories>
        <repository>
            <!-- id一致 -->
            <id>maven-public</id>
            <!-- 私服 url -->
            <url>http://192.168.150.101:8081/repository/maven-public/</url>
            <releases>
            	<enabled>true</enabled>
            </releases>
            <snapshots>
            	<enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>
</profile>
```

**3. IDEA的maven聚合工程（或者父工程）的pom文件中配置上传（发布）地址(直接在xxx-parent中配置发布地址)**

```xml
<distributionManagement>
    <!-- release版本的发布地址 -->
    <repository>
        <id>maven-releases</id>
        <url>http://192.168.150.101:8081/repository/maven-releases/</url>
    </repository>

    <!-- snapshot版本的发布地址 -->
    <snapshotRepository>
        <id>maven-snapshots</id>
        <url>http://192.168.150.101:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```



**配置完成之后，我们就可以在项目中执行 `deploy` 生命周期，将项目发布到私服仓库中。** 