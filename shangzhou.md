# 序言

商周Z10中台开发规范包括代码结构规范、java代码编写规范、数据库设计规范等。

# 中台介绍

## 架构说明

### 总体架构



### 业务架构

![image-20220514092705220](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220514092705220-20220514094907881.png)

### 技术架构

![image-20220514095047351](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220514095047351.png)

### 集成架构

![image-20220514100838934](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220514100838934.png)

## 环境准备

### 软件准备

#### SDK

#### Idea

#### MySQL

#### Maven

### 环境配置

#### Idea配置

#### MySQL配置

#### 代码导入

### 项目构建

#### 目录结构

通过macula-tools-archtype可以自动生成如下项目结构：

![tutorials-project-tree.png](https://macula.dev/macula-reference/images/chapter1/tutorials-project-tree.png)

macula tools默认创建的项目结构是典型的三层架构，包含了如下模块：



- **macula-samples**：项目最外层的模块，主要用于打包分发项目；
- **macula-samples-parent**：所有模块的父模块，提供了公共依赖项的配置和其它子模块依赖项的版本设置；
- **macula-samples-repository**：该模块包含domain和repository两个package，domain package存放与数据库表的映射类，repository存放数据库的存取操作类，domain是基于JPA和Hibernate的，repository主要采用spring-data；
- **macula-samples-service**：该模块主要包含业务逻辑层，通过调用repository来完成数据库的读写；
- **macula-samples-admin/front/mobile**：界面层模块，包含所有的Controller、Freemarker模块、Javascript文件，Controller主要基于Spring MVC，freemarker和javascript基于Macula UI(Mower)的规范编写页面；
- **macula-samples-webapp**：war包的打包模块，该模块包含了全局配置文件，javascript、css、image等静态资源文件，web.xml等J2EE WEB模块的标准文件，并负责将上述模块打包成一个war发布。

**例 3.1. macula-samples-parent中唯一的文件pom.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.macula.samples</groupId>
    <artifactId>macula-samples-parent</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>macula-samples-parent</name>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <jdkLevel>1.7</jdkLevel>
        <jvmargs>-XX:MaxPermSize=384m -Xms512m -Xmx1024m</jvmargs>
        <macula.version>3.0.0-SNAPSHOT</macula.version>
        <macula.plugins.version>3.0.0-SNAPSHOT</macula.plugins.version>
        <mockito.version>1.9.5</mockito.version>
        <junit.version>4.12</junit.version>
        <hsql.version>2.3.2</hsql.version>
        <spring.version>4.1.7.RELEASE</spring.version>
        <servlet.version>3.0.1</servlet.version>
        <jta.version>1.1</jta.version>
        <oracle.version>11.2.0.3.0</oracle.version>
        <mysql.version>5.1.35</mysql.version>
        <liquibase.version>3.0.8</liquibase.version>
    </properties>
    .....
</project>
```

在macula-samples-parent的maven设置中，指定了maven第三方包获取点：

```xml
<repositories>
    <repository>
        <id>macula-repo</id>
        <name>macula-repo</name>
        <url>http://maven.infinitus.com.cn:8081/nexus/content/groups/public</url>
    </repository>
</repositories>
```

当前所有macula平台所需要使用的第三方包均可以在上述获取点获得，为了规范第三方包的使用以及避免版本冲突，在进行依赖macula平台开发的业务系统中，只允许依赖macula平台的模块，对于需要依赖第三方包的，需要提交审批，审批通过后，将在上述maven获取点能获取到该第三方报，业务系统方能使用，否则不允许使用。

下面以macula-samples下的包为例，介绍其目录及文件的含义。

1. jar模块目录结构

   在maven创建项目后，将创建目录：

   - src/main/java ：该目录放置java的主要开发代码，即最终运行需要的java类，这个目录的内容最终将打包到jar中。

   - src/main/resources ：该目录放置除java带的其他资源文件，如xml、properties文件等，这个目录的内容最终也将被打包到jar中。

   - src/test/java ：该目录主要存放JUnit测试的java代码，用于测试阶段的代码，这个目录的内容将不打包到jar中。

   - src/test/resources ：该目录存放测试下需要使用的资源文件，如xml、properties文件等，这个目录的内容也不会打包到jar文件中。

     **重要**

     *需要特别强调的是，测试文件必须放在test目录下，以降低打包jar文件的大小以及代码结构的清晰度，对于编写的JUnit用例，必须放在测试目录。另外对于src/main/resources中存放文件的目录结构（包括Spring配置文件的放置、Freemarker模块文档的放置等），将在其他相应章节中介绍，这里主要介绍大方向的目录结构。*

1. war模块目录结构

   war模块可通过maven-wtp插件，将war模块直接发布到Eclipse定义的Server中，对于war模块，除了具备jar模块的文档结构外，另外增加了src/main/webapp目录，该目录按标准的J2EE应用的目录格式和命名方式。特别的，对于src/main/webapp下的目录结构，也需要严格按照下列命名：

   - webapp/META-INF ：下面放置该模块的一些自描述信息。
   - webapp/resources ：该目录下放置静态内容信息，包括图片文件、javascript文件等。
   - webapp/WEB-INF ：该目录为标准的J2EE要求目录。

#### 自定义目录

​		在应用开发中，需要自定义目录的情况较少，如无特殊需要，尽量减少自定义目录的情况，自定义目录会增加项目组沟通成本以及维护成本。

​		对于自定义目录的情况大致有：

- 为多种环境创建不同的配置
  这种情况下，主要使用src/resources/configs/xxx目录创建同名文件，并通过启动时加入-Dmacula.profile=xxx来切换配置文件。
- webapp目录下增加其他静态文件
  对war模块，对于webapp下需要载入的大量的静态文件以及脚本文件，可能会加入自定义目录。

#### 配置文件

​		macula平台下涉及的文件包括：

- macula.properties：开发平台配置文件
- Spring配置文件，将在配置文件章节介绍
- log4j.properties：log4j的配置文件
- freemarker.properties：freemarker模版的配置文件

#### 依赖包及版本

​		商周开发平台自身依赖了大量的第三方包，在业务系统开发中，使用依赖basic平台的模块即可，对于具体的依赖包及版本，可查看basic-framework-components-dependencies模块中定义的pom.xml文件，如需额外第三方模块需报备统一在dependencies维护版本。

### 发布说明

#### SpringBoot

​		SpringBoot 框架只提供了一套基于可执行 jar 包（executable jar）格式的标准发布形式，但并没有对部署做过多的界定，而且为了简化可执行 jar 包的生成，SpringBoot 提供了相应的 Maven 项目插件。

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
      <!-- 其他插件定义 -->
    </plugins>
</build> 
```

​		然后只要我们运行 mvn install，当前 SpringBoot 项目就会被打包成一个包含了其所有项目依赖以及该项目本身的可执行 jar 包，通过 scp 或者 rsync 等方式将这个可执行 jar 包部署到目标环境的服务器之后。

就可以通过 java-jar your-project.jar 启动 SpringBoot 应用了，如目标环境的服务器为docker容器环境，通过构建Docker容器启动SpringBoot 应用。SpringBoot 提供了相应的 Dcoker 项目插件

```xml
<build>
    <plugins>
        <plugin>
             <groupId>com.spotify</groupId>
             <artifactId>docker-maven-plugin</artifactId>
        </plugin>
      <!-- 其他插件定义 -->
    </plugins>
</build> 
```



#### Jenkins

#### 码云

#### Docker

## 核心技术

### 配置文件

基于Springboot开发的项目，您需要关注的配置文件位于您的resources工程下，包括：

- **Spring配置**

  application.yml springboot公共配置
  application-xxx.yml 数据库相关、Redis相关等需要连接外部资源的配置，xxx代表运行环境标识，本地环境local、开发环境dev、测试环境test、预发布环境sit、生产环境prd。

- **消息配置**
  META-INF/message/basic.framework.components.message 配置消息文件路径
  META-INF/i18n/xxx.properties 消息文件配置

### 项目集成

- **模块本地集成**
- **模块远程集成**



### 数据存取层

在数据存取层，与传统的DAO层的实现不同，这里引入Basic框架，可实现部分接口只定义接口，而不用编写实现，可减少编码的工作量。

- **Basic数据存取接口**

Basic数据存取接口BasicCrudDao默认可实现下列功能：

```java
/**
 * 增删改查公共方法
 *
 * @param <T> 泛型对象
 * @author panda
 */
public interface BasicCrudDao<T> extends BasicReadDao<T> {


    /**
     * 新增
     *
     * @param entity 实体对象
     * @return 修改的数据条数
     */
    int insert(T entity);

    /**
     * 根据主键更新对象
     *
     * @param entity 实体对象
     * @return 修改的数据条数
     */
    int updateById(T entity);

    /**
     * 根据主键删除数据
     *
     * @param pk 主键
     * @return 删除的数据条数
     */
    int deleteById(Serializable pk);


    /**
     * 批量新增数据
     *
     * @param list 待新增的数据
     * @return 新增的条数
     */
    int insertBatch(List<T> list);


}
```

BasicCrudDao继承BasicReadDao接口类，该接口类默认实现

```java
/**
 * 读取公共方法
 *
 * @author panda
 * @param <T> 泛型对象
 */
public interface BasicReadDao<T> {


    /**
     * 根据主键获数据
     *
     * @param pk 主键
     * @return 所获取的对象
     */
    T getById(Serializable pk);

    /**
     * 根据查询对象获取实体对象
     *
     * @param criteria 查询对象
     * @param <C>      查询对象泛型
     * @return 所获取的对象
     */
    <C> T getEntity(C criteria);

    /**
     * 根据主键和字段名获数据
     *
     * @param pk        主键
     * @param fieldName 需要获取的字段
     * @param <V>       返回的对象泛型
     * @return 查询后的数据
     */
    <V> V getValueById(@Param("id") Object pk, @Param("fieldName") String fieldName);


    /**
     * 获取所有数据
     *
     * @return 返回的数据
     */
    List<T> getEntityList();

    /**
     * 根据查询对象获取数据
     *
     * @param criteria 查询对象
     * @param <C>      查询对象泛型
     * @return 返回的数据
     */
    <C> List<T> getEntityList(C criteria);


    /**
     * 获取所有数据
     *
     * @return 返回的数据
     */
    Long getEntityCount();

    /**
     * 根据查询对象获取数据
     *
     * @param criteria 查询对象
     * @param <C>      查询对象泛型
     * @return 返回的数据
     */
    <C> Long getEntityCount(C criteria);


    /**
     * 分页查询数据
     *
     * @param pageable 分页对象
     * @return 返回的数据
     */
    Page<T> getEntityListForPage(Pageable pageable);

    /**
     * 根据查询条件分页查询数据
     *
     * @param criteria 查询条件
     * @param pageable 分页条件
     * @param <C>      查询条件泛型
     * @return 返回的数据
     */
    <C> Page<T> getEntityListForPage(C criteria, Pageable pageable);


}
```

- **接口方法**

通过Spring的@Repository注解，可将上述的BasicCrudDao定义的接口直接转化为spring bean，除开如上默认定义好的接口方法，如需额外接口方法，则需要自行实现方法，实现规则参照Mybatis,，通过Mybatis注解或xml来实现。

例如用户类查询接口

```java
@Repository
public interface UserDao extends BasicCrudDao<User> {
    @Select("SELECT * from user where username =#{username}")
    User getUser(@Param("username") String username);
}
```

- **默认方法参数**

1. **BaseCriteria**

通用查询参数构建方式

```java
BaseCriteria<?> criteria = EntityCriteria.build();
```

2. **PageableDto**

通用分页参数构建方式

```java
Pages.loadPageable(pageableDto)
```

### 业务服务层

平台自身是基于Spring作为开发基础，在Spring Framework的基础上，增加的一些规则标准与规范，

业务服务层通过Spring Bean来定义，其开发本身与一般的基于Spring的J2EE开发一致，下面就商周平台所做出的一些要求做一些介绍。

#### 易读性接口

业务服务层涉及的代码量在整个开发中占据非常大的比例，为了保证程序代码的整洁和易读性，对服务层的方法都必须按功能、用途、数据访问情况等方式，分解到一个或多个接口中，并在命名上具备相当的可读性，使代码阅读者能通过接口名称以及方法名称直观的了解到业务方法的目的。

#### 参数校验

业务服务层最总的调用者可能来自Controller层，也可能来自其他Service层，所以对于服务层方法的输入参数，需要进行合法性检查，尽快将不合法的输入参数以异常的方式抛出，而不是在程序边运行边检查，这样容易造成开销的增大，同时也失去了结构的严谨性以及代码的可读性。

#### 事务配置

对于服务层的事务，框架通过aop拦截实现，事务控制通过方法名前缀进行控制，因此需要在Config中实现TransactionInterceptor和BeanNameAutoProxyCreator。

例如：

```java
@Bean(name = "aopTransactionInterceptor")
public TransactionInterceptor aopTransactionInterceptor(DataSourceTransactionManager transactionManager) {
    TransactionInterceptor transactionInterceptor = new TransactionInterceptor();
    transactionInterceptor.setTransactionManager(transactionManager);
    Properties properties = new Properties();
    properties.setProperty("remove*", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    properties.setProperty("delete*", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    properties.setProperty("add*", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    properties.setProperty("insert*", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    properties.setProperty("save*", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    properties.setProperty("update*", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    properties.setProperty("publish*", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    properties.setProperty("register*", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    properties.setProperty("detail*", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    properties.setProperty("change*", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    properties.setProperty("*", "PROPAGATION_SUPPORTS");
    properties.setProperty("handleProcessUpdate", "PROPAGATION_REQUIRED,ISOLATION_DEFAULT,-Exception");
    transactionInterceptor.setTransactionAttributes(properties);
    return transactionInterceptor;
}

@Bean
public BeanNameAutoProxyCreator transactionAutoProxy(List<MethodInterceptor> methodInterceptor) {
    BeanNameAutoProxyCreator autoProxy = new BeanNameAutoProxyCreator();
    autoProxy.setProxyTargetClass(true);
    autoProxy.setBeanNames("*ServiceImpl");
    autoProxy.setBeanNames("*Controller");
    autoProxy.setInterceptorNames("aopTransactionInterceptor");
    return autoProxy;
}
```

#### 服务层的异常

服务层主要是调用Dao，处理事务，这一层不需要特殊处理异常，如果异常会影响你的业务逻辑，则需要try然后按照您的逻辑编写代码，如果是一些校验类的异常，可以直接抛出，如果需要自定义异常则实例化BaseException。

### 接口服务层

在商周开发平台下，建议使用Spring MVC + Rest的方式来实现接口服务层，所有的接口都采用Ajax方式输入输出。

### 时间与格式

时间传输统一采用yyyy-MM-dd HH:mm:ss格式，如需要自定义传输格式，则需要使用注解@DateTimeFormat

## 框架介绍

### BASIC基础组件

- **basic-framework-components-dependencies **

依赖管理组件	用于管理整个项目的依赖的版本。

- **basic-framework-components-core**

 BASIC核心组件	该组件为基础核心组件，定义了BaseException等常用异常类，包含IdUtil、Jsons、Strings、Springs等常用的工具类。

- **basic-framework-components-mybatis**

Mybatis扩展组件	提供基于Mybatis的功能扩展，包括如下功能：	

1. 基础增删改查接口： BasicCrudDao<T>	
2. 条件构造抽象类： BaseCriteria<T>，支持分页和排序检索	
3. 多数据源支持接口类： Dialect（ MySQLDialect 、 OracleDialect ）

- **basic-framework-components-message** 

国际化Message组件	管理项目中资源消息文件，提供加载多个i18n资源文件的功能扩展。

- **basic-framework-components-mvcswagger**

Swagger2简化配置组件	该组件主要用于简化Swagger的配置	引入该组件后，只需要在Application类头部加入@EnableSwagger注解即可使用Swagger文档管理工具同时支持sunInfo配置Swagger下拉分组。

- **basic-framework-components-sms**

短信扩展组件	该组件整合多个短信平台功能接口，可以通过相同的配置方式、API调用方式调用不同的短信平台功能	目前支持的平台有：聚合短信平台（juhe）、互亿无线平台（huyi）、漫道平台（zucp）、阿里大于短信平台（alidayu）、亿美短信中心（yimei）

- **basic-framework-components-cache**

缓存组件	尚在实现中的组件，提供多级缓存相关的功能（本地-redis-数据库）

- **basic-framework-components-mp**

微信公众平台组件,该组件提供了简化调用微信公众平台相关接口功能，提供了多种accessToken加载机制。整合的微信公众平台组件包括如下：

    1. **BaseComponent**：授权、获取openId、获取accessToken等
    2. **CustomerServiceComponent**：多客服组件
    3. **DataComponent**：数据统计组件
    4. **JsSdkComponent**：JsSdk组件
    5. **MaterialComponent**：素材组件
    6. **MenuComponent**：菜单组件
    7. **MessageComponent**：模板消息组件
    8. **QrCodeComponent**：二维码组件
    9. **UserComponent**：用户组件

- **basic-framework-components-pay**

支付组件	该组件整合多个支付平台功能接口，可以通过相同的配置方式、API调用方式调用不同的支付平台功能	目前支持的平台有：支付宝支付（Alipay）、微信支付（Wepay）

- **basic-framework-components-proxy**

动态代理组件	提供一个类似Nginx功能的代理组件

### BASIC基础能力

- **basic-framework-ability-log-parent**

日志记录能力，提供了非侵入式的日志记录和行为动态处理能力	拦截Controller请求，分析入参和返回结果。通过在数据库中配置的业务引擎规则，输出格式化之后的日志数据到数据源中。

- **basic-framework-ability-config-parent**

自动加载和缓存系统配置项的能力

- **basic-framework-ability-sms-parent**	

短信中心能力，提供系统短信平台集成的管理能力

- **basic-framework-ability-admin-parent**

提供系统应用监控和配置管理的能力	支持对多应用的集群管理，引入该能力后，通过’{appHost:appPort}’ 和默认密码访问

- **basic-framework-ability-queue-parent**

提供消息中心实现的能力 	

## 技术标准

### Java代码规范

Java规范基本可以执行《阿里巴巴JAVA开发手册》，在这个基础上
