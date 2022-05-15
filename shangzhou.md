# 序言

商周Z10中台开发规范包括代码结构规范、java代码编写规范、数据库设计规范等。

# 中台介绍

## 架构说明

### 总体架构

![image-20220515191454068](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220515191454068.png)

### 业务架构

![image-20220514092705220](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220514092705220-20220514094907881.png)

### 技术架构

![image-20220514095047351](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220514095047351.png)

### 集成架构

![image-20220514100838934](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220514100838934.png)

## 环境准备

### 软件准备

#### SDK

商周框架现阶段所有的代码都采用JDK8进行编译发布，因此本地环境应先行准备好jdk1.8环境，同时由于jdk已经商业，因此我们在开发过程中现在使用开源版的openjdk。下载地址：https://adoptopenjdk.net/

#### Idea

1. 设置File Encodings为UTF-8	
2. 设置Maven的Home目录和settings.xml文件路径	
3. 安装常用的插件

下载地址：https://www.jetbrains.com/idea/

#### Navicat

1. 数据库连接和管理工具，可以连接多种数据库	
2. 进行数据同步、数据导出、不同类型数据库之间的数据迁移

下载地址：https://www.navicat.com/cht/products

#### Postman

1. 接口测试工具，用于代码写好后自测接口功能	
2. 相比Swagger，可以定义测试变量、保存测试参数等等高级功能

下载地址：https://www.postman.com/downloads/

#### SourceTree

1. git版本管理软件	
2. gitflow：develop、master、feature、hotfix、release

下载地址：https://www.sourcetreeapp.com/

#### SecureCRT
1. SSH连接服务器的工具
2. 类似的软件还有Xshell、putty等

下载地址：https://www.vandyke.com/download/index.html

#### FileZilla
1. 和服务器之间进行文件传输的工具，支持SFTP、FTP
2. 类似的软件还有WinSCP等

下载地址：https://filezilla-project.org/download.php?type=client

#### Redis Desktop Manager（RDM）
Redis连接服务器的工具

下载地址：https://resp.app/

#### Teamviewer
兼容Windows、Mac、Linux的远程桌面控制软件

下载地址：https://www.teamviewer.com/en-us/download/

#### Xmind ZEN
思维导图工具，可以用于记录平时开发过程中遇到的问题、新的知识点、检索的网络资料

下载地址：https://www.xmind.net/download/

### 环境配置

#### Maven

配置maven配置文件settings.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
        <mirrors>
            <mirror>
                <id>shangzhou-repository</id>
                <mirrorOf>*</mirrorOf>
                <name>shangzhou-repository</name>
                <url>http://124.70.147.136:8081/repository/maven-public/</url>
            </mirror>
        </mirrors>
    <servers>
        <server>
            <id>releases</id>
            <username>admin</username>
            <password>xxxxxx</password>
        </server>
        <server>
            <id>snapshots</id>
            <username>admin</username>
            <password>xxxxxx</password>
        </server>
        <server>
            <!--maven的pom中可以根据这个id找到这个server节点的配置-->
            <id>docker-huawei</id>
            <!--这里是在阿里云注册的账号-->
            <username>cn-east-3@GVJ2I4JYIYDHV1P400TP</username>
            <!--这里是在阿里云注册的密码-->
            <password>xxxxxx</password>
            <configuration>
                <!--这是在阿里云注册时填写的邮箱-->
                <email>wusuoming@hotmail.com</email>
            </configuration>
        </server>
    </servers>
    <profiles>
<profile>
            <id>shangzhou</id>
            <properties>
                <altReleaseDeploymentRepository>
                    shangzhou-releases::default::http://124.70.147.136:8081/repository/maven-releases/
                </altReleaseDeploymentRepository>
                <altSnapshotDeploymentRepository>
                    shangzhou-snapshots::default::http://124.70.147.136:8081/repository/maven-snapshots/
                </altSnapshotDeploymentRepository>
            </properties>
            <repositories>
                <repository>
                    <id>shangzhou-releases</id>
                    <url>http://124.70.147.136:8081/repository/maven-releases/</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
                <repository>
                    <id>shangzhou-snapshots</id>
                    <url>http://124.70.147.136:8081/repository/maven-snapshots/</url>
                    <releases>
                        <enabled>false</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>shangzhou-releases</id>
                    <url>http://124.70.147.136:8081/repository/maven-releases/</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
                <pluginRepository>
                    <id>shangzhou-snapshots</id>
                    <url>http://124.70.147.136:8081/repository/maven-snapshots/</url>
                    <releases>
                        <enabled>false</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>
        <activeProfiles>
        <activeProfile>shangzhou</activeProfile>
    </activeProfiles>
</settings>
```

#### Idea配置教程

1. IntelliJ IDEA 中配置

Maven将本地安装的 Maven 配置到 IntelliJ IDEA 中操作步骤如下网页地址所示：http://c.biancheng.net/maven2/idea-maven-config.html

2. IDEA新建Maven项目

在 IntelliJ IDEA 中新建 Maven 项目操作步骤如下网页地址所示：http://c.biancheng.net/maven2/idea-build-maven.html

3. IDEA 导入Maven 项目

相比新建 Maven 项目，实际工作中使用更多的是将已有的 Maven 项目导入 IntelliJ IDEA 中，具体步骤如下网页地址所示：http://c.biancheng.net/maven2/idea-import-maven.html

### 项目构建

#### 目录结构

商周创建的项目结构是典型的三层架构，包含了如下模块：

- **szc-server-xxx-parent** 
  所有模块的父模块，提供了公共依赖项的配置和其它子模块依赖项的版本设置
  - **szc-server-xxx-boot**
  模块启动项目，负责打包生成启动jar文件，如该模块无需独立部署，则该模块可不用构建
  - **szc-server-xxx-rest-parent**
  该模块为rest模块父工程，提供rest模块公共依赖和版本
    - **szc-server-xxx-rest-controller**
  该模块主要包含业务控制层，通过调用Service来完成数据库的读写
    - **szc-server-xxx-rest-register**
  该模块主要包含业务控制层外放接口，和Controller模块一一对应
  - **szc-server-service-xxx-parent**
    该模块为service模块父工程，提供service模块公共依赖和版本
    - **szc-server-service-xxx-service**
    该模块主要包含业务逻辑层，通过调用Dao来完成数据库的读写
    - **szc-server-service-xxx-facade**
    该模块主要包含业务逻辑层外放接口，和service模块一一对应

其中 szc-server-xxx-parent中唯一的文件pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.shangzhou.szc</groupId>
    <version>1.0.0-SNAPSHOT</version>
    <modules>
        <module>szc-server-xxx-boot</module>
        <module>szc-server-xxx-rest-parent</module>
        <module>szc-server-service-xxx-parent</module>
    </modules>

    <artifactId>szc-server-xxx-parent</artifactId>
    <packaging>pom</packaging>
    <dependencies>
    </dependencies>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <artifactId>basic-framework-components-dependencies</artifactId>
                <groupId>com.shangzhou.framework</groupId>
                <version>1.1-SNAPSHOT</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <distributionManagement>
        <snapshotRepository>
            <id>snapshots</id>
            <url>http://124.70.147.136:8081/repository/maven-snapshots/</url>
        </snapshotRepository>
        <repository>
            <id>releases</id>
            <url>http://124.70.147.136:8081/repository/maven-releases/</url>
        </repository>
    </distributionManagement>
		<repositories>
        <repository>
            <id>nexus</id>
            <name>Nexus Repository</name>
            <url>http://124.70.147.136:8081/repository/maven-public/</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
            <releases>
                <enabled>true</enabled>
            </releases>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>nexus</id>
            <name>Nexus Plugin Repository</name>
            <url>http://124.70.147.136:8081/repository/maven-public/</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
            <releases>
                <enabled>true</enabled>
            </releases>
        </pluginRepository>
    </pluginRepositories>    
  	<build>
        <defaultGoal>compile</defaultGoal>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
                <version>3.0.1</version>
                <executions>
                    <execution>
                        <id>attach-sources</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <compilerArgument>-parameters</compilerArgument>
                    <encoding>UTF-8</encoding>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.1.2</version>
                <configuration>
                    <archive>
                        <manifestEntries>
                            <timestamp>${maven.build.timestamp}</timestamp>
                            <version>${project.version}</version>
                            <groupId>${project.groupId}</groupId>
                            <artifactId>${project.artifactId}</artifactId>
                            <buildOsName>${os.name} ${os.arch} ${os.version}</buildOsName>
                            <Os-Name>${os.name}</Os-Name>
                            <Os-Arch>${os.arch}</Os-Arch>
                            <Os-Version>${os.version}</Os-Version>
                        </manifestEntries>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

其中在 szc-server-xxx-parent的maven设置中，指定了maven第三方包获取点：

```xml
    <distributionManagement>
        <snapshotRepository>
            <id>snapshots</id>
            <url>http://124.70.147.136:8081/repository/maven-snapshots/</url>
        </snapshotRepository>
        <repository>
            <id>releases</id>
            <url>http://124.70.147.136:8081/repository/maven-releases/</url>
        </repository>
    </distributionManagement>
		<repositories>
        <repository>
            <id>nexus</id>
            <name>Nexus Repository</name>
            <url>http://124.70.147.136:8081/repository/maven-public/</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
            <releases>
                <enabled>true</enabled>
            </releases>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>nexus</id>
            <name>Nexus Plugin Repository</name>
            <url>http://124.70.147.136:8081/repository/maven-public/</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
            <releases>
                <enabled>true</enabled>
            </releases>
        </pluginRepository>
    </pluginRepositories>  
```

其中在 szc-server-xxx-parent的maven设置中，指定了maven第三方组件依赖版本控制：

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <artifactId>basic-framework-components-dependencies</artifactId>
            <groupId>com.shangzhou.framework</groupId>
            <version>1.1-SNAPSHOT</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

#### 配置文件

​		商周平台下涉及的文件包括：

- **Spring配置文件**    详情请见配置文件章节
- **消息配置**    框架异常编码统一处理，详情请见配置文件章节

#### Boot配置

- **创建启动类**

```java
@EnableScheduling
@EnableAutoConfiguration(exclude = {JpaRepositoriesAutoConfiguration.class})
@EnableMessageSource
@EnableSwagger(resourcePackage = "com.shangzhou.szc", title = "xxx中心服务端接口", version = "1.0.0", contact = "商周研发中心", description = "后端服务接口由商周研发中心提供支持~",subInfo = {@SubSwagger(resourcePackage = "com.lit.szc.rest.xxx.controller", groupName = "xxx中文说明"),})
@Import({ApplicationConfig.class})
@SpringBootConfiguration
public class CardCenterApplication {

    public static void main(String[] args) {

        long beginTime = System.currentTimeMillis();

        SpringApplication.run(CardCenterApplication.class, args);

        long endTime = System.currentTimeMillis();

        System.out.println("------------------------------------------ -----------------------------\n" +
                "-----------------------------------------------------------------------\n" +
                "                               _     _\n" +
                "                              ( \\---/ )\n" +
                "                               ) . . (\n" +
                "-------------------------,--._(___Y___)_,--.---------------------------\n" +
                "                         `--'           `--'\n" +
                "                         \t"+ ApplicationUtils.getProperty("application.name")+"启动成功                         \n" +
                " 启动耗时: " + (endTime - beginTime) + "毫秒 \n" +
                " 端口: "+ ApplicationUtils.getProperty("server.port") +" \n" +
                " 配置文件环境: " + ApplicationUtils.getProperty("spring.profiles.active") +"\n" +
                " Basic监控系统访问地址: http://"+ NetworkUtils.getHostName() +":"+ApplicationUtils.getProperty("server.port")+"/" +ApplicationUtils.getProperty("management.admin.name")+ " \n" +
                "-----------------------------------------------------------------------\n" +
                "  \n");
    }
}
```

其中配置部分启动类参数

**@EnableScheduling**    定时任务生效配置

**@EnableMessageSource**     消息转换生效配置

**@EnableSwagger**    swagger生效配置

**导入ApplicationConfig.class**    导入公共应用配置

- **创建ApplicationConfig**

```java
@Configurable
@ComponentScan("com.lit.szc")
@Import({CommonConfig.class, DbConfig.class, TaskExecutorConfig.class, LitMybatisConfig.class})
public class ApplicationConfig {
 
}
```

其中导入部分配置

**CommonConfig** 框架自动配置的公共参数配置

**LitMybatisConfig** 框架自动配置的Mybatis参数配置

**DbConfig** 数据库操作相应配置，各工程自行配置，具体配置示例如下：

```java
@Configurable
@Import({CommonDbConfig.class})
public class DbConfig {

    @Bean(name = "aopTransactionInterceptor")
    public TransactionInterceptor transactionInterceptor(DataSourceTransactionManager transactionManager) {
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
        properties.setProperty("*", "PROPAGATION_SUPPORTS");
        //审核流程事务
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
}
```

**TaskExecutorConfig** 定时任务操作相应配置，各工程自行配置，如该工程使用定时任务则必须配置，具体配置示例如下：

```java
@Configuration //声明配置类
@EnableAsync //开启异步任务支持
public class TaskExecutorConfig {

    /**
     * 自定义异步线程池
     *
     * @return
     */
    @Bean
    public AsyncTaskExecutor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setThreadNamePrefix("szc-Executor");
        executor.setMaxPoolSize(10);
        // 设置拒绝策略
        executor.setRejectedExecutionHandler((r, executor1) -> {
            // .....
        });


        return executor;
    }


}
```

配置pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.shangzhou.szc</groupId>
    <version>1.0.0-SNAPSHOT</version>
    <artifactId>szc-server-yire-all-boot</artifactId>
    <packaging>jar</packaging>
    <!-- 阿里云Docker仓库配置 开始-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.build.timestamp.format>yyyy-MM-dd HH:mm:ss</maven.build.timestamp.format>
        <docker.repository>swr.cn-east-3.myhuaweicloud.com</docker.repository>
        <docker.registry.name>szzl</docker.registry.name>
        <build.env>DEV</build.env>
    </properties>
    <!-- 阿里云Docker仓库配置 结束-->
    <distributionManagement>
        <snapshotRepository>
            <id>snapshots</id>
            <url>http://124.70.147.136:8081/repository/maven-snapshots/</url>
        </snapshotRepository>
        <repository>
            <id>releases</id>
            <url>http://124.70.147.136:8081/repository/maven-releases/</url>
        </repository>
    </distributionManagement>
    <repositories>
        <repository>
            <id>nexus</id>
            <name>Nexus Repository</name>
            <url>http://124.70.147.136:8081/repository/maven-public/</url>
            <snapshots>
                <enabled>true</enabled>
                <updatePolicy>always</updatePolicy>
            </snapshots>
            <releases>
                <enabled>true</enabled>
                <updatePolicy>always</updatePolicy>
            </releases>
        </repository>
    </repositories>

    <pluginRepositories>
        <pluginRepository>
            <id>nexus</id>
            <name>Nexus Plugin Repository</name>
            <url>http://124.70.147.136:8081/repository/maven-public/</url>
            <snapshots>
                <enabled>true</enabled>
                <updatePolicy>always</updatePolicy>
            </snapshots>
            <releases>
                <enabled>true</enabled>
                <updatePolicy>always</updatePolicy>
            </releases>
        </pluginRepository>
    </pluginRepositories>                    
  <!--第三方jar版本控制-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <artifactId>basic-framework-components-dependencies</artifactId>
                <groupId>com.shangzhou.framework</groupId>
                <version>1.1-SNAPSHOT</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <build>
        <defaultGoal>compile</defaultGoal>
        <plugins>
              <!--打包发布忽略配置-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-deploy-plugin</artifactId>
                <version>2.8.2</version>
                <configuration>
                    <skip>true</skip>
                </configuration>
            </plugin>
             <!--打包发布源码配置-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
                <version>3.0.1</version>
                <executions>
                    <execution>
                        <id>attach-sources</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
             <!--打包发布提取方法参数名配置-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <compilerArgument>-parameters</compilerArgument>
                    <encoding>UTF-8</encoding>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
             <!--打包发布jar配置-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.1.2</version>
                <configuration>
                    <archive>
                        <manifestEntries>
                            <timestamp>${maven.build.timestamp}</timestamp>
                            <version>${project.version}</version>
                            <groupId>${project.groupId}</groupId>
                            <artifactId>${project.artifactId}</artifactId>
                            <buildOsName>${os.name} ${os.arch} ${os.version}</buildOsName>
                            <Os-Name>${os.name}</Os-Name>
                            <Os-Arch>${os.arch}</Os-Arch>
                            <Os-Version>${os.version}</Os-Version>
                        </manifestEntries>
                    </archive>
                </configuration>
            </plugin>
						<!--打包发布Springboot配置-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>1.5.22.RELEASE</version>
                <configuration>
                    <mainClass>com.lit.szc.boot.all.YireAllCenterApplication</mainClass>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
          	<!--打包发布Docker配置-->
            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>docker-maven-plugin</artifactId>
                <version>1.1.1</version>
                <configuration>
                    <imageName>
                        ${docker.repository}/${docker.registry.name}/${project.artifactId}:${build.env}${project.version}
                    </imageName>
                    <baseImage>openjdk:8-alpine</baseImage>
                    <env>
                        <TZ>Asia/Shanghai</TZ>
                    </env>
                    <!--远程docker服务地址-->
                    <cmd>java -jar ${project.build.finalName}.jar</cmd>
                    <resources>
                        <resource>
                            <targetPath>/</targetPath>
                            <directory>${project.build.directory}</directory>
                            <include>${project.build.finalName}.jar</include>
                        </resource>
                    </resources>
                    <serverId>docker-huawei</serverId>
                    <registryUrl>${docker.repository}</registryUrl>
                    <pushImage>true</pushImage>
                </configuration>
            </plugin>
        </plugins>
    </build>
    <!-- 项目构建脚本 开始 -->
    <!-- 项目构建脚本 结束 -->

    <dependencies>
        <!-- 子工程相关依赖 开始 -->
       
        <!-- 子工程相关依赖 结束 -->

        <!-- Springboot 开始 -->

        <!-- Springboot 结束 -->


        <!-- Spring系列jar包 开始-->

        <!-- Spring系列jar包 结束-->

        <!-- basic2.0系列jar包 开始-->

        <!-- basic2.0系列jar包 结束-->

        <!-- mysql连接驱动 开始-->
        
    </dependencies>


</project>

```

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

- **创建分组视图**

按模块创建相应的分组视图，选择列表视图

![image-20220515192009699](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220515192009699.png)

- **创建编译任务**

1. 按模块创建maven构建任务，配置相应的代码地址，构建代码。可复制相同的构建任务进行复制。，如下图：

![image-20220515192300788](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220515192300788.png)

2. 配置源码管理，如下图：

![image-20220515193141204](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220515193141204.png)

3. 配置gitee代码钩子，如下图：

![image-20220515193611879](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220515193611879.png)

- **构建发布**

当代码构建成功后，如需独立发布，可创建独立发布任务，并配置对应的任务名称，进行构建发布，如不需要独立构建发布，则配置对应的合并构建任务，如下图：

![image-20220515194019003](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220515194019003.png)
-  **创建独立发布任务**

1. 配置构建任务，根据maven中的配置打包相应的镜像，如下图：

![image-20220515194941712](https://cdn.jsdelivr.net/gh/wusuoming/image@master/uPic/image-20220515194941712.png)

2. 配置远程执行脚本
```shell
docker stop  szc-server-xxx-boot
docker rm szc-server-xxx-boot
docker rmi swr.cn-east-3.myhuaweicloud.com/szzl/szc-server-xxx-boot:DEV1.0.0-SNAPSHOT
docker run   -d     --name szc-server-xxx-boot --restart=always -v /app/szc/xxx/logs:/logs  -v /app/szc/xxx/files:/app/szc/xxx   -p 7970:7970  swr.cn-east-3.myhuaweicloud.com/szzl/szc-server-xxx-boot:DEV1.0.0-SNAPSHOT /bin/sh -c 'java -jar -Xmx512m -Xms512m -Dspring.profiles.active=sit  szc-server-xxx-boot-1.0.0-SNAPSHOT.jar'
```

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

本地模块集成需要将中心模块或者应用模块的Controller直接依赖到启动类所在工程的pom文件，同时在调用所在工程的pom文件中依赖register即可。

- **模块远程集成**

远程模块集成需要在调用所在工程的pom文件中依赖register，同时在启动类所在的springboot配置文件中配置如下配置参数：

```yaml
net:
  luohuasheng:
    rest:
      register:
        card:
          url: http://119.3.122.149:7280
          scan: com.lit.szc.rest.xxx.register
          convent: com.lit.szc.server.common.proxy.convent.MessageResult
```

- **中心API集成**

中心API集成则需要通过服务网关鉴权，然后调用网关转发的相应接口。



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

## 平台能力介绍

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

### 企业版中台服务	

#### 技术中台

- **szc-server-common-parent**

**公共服务模块**	公共组件、工具类等代码

- **szc-server-gateway-parent**

**服务网关**	权限认证、中台接口统一入口

- **szc-server-task-parent**

**任务调度中心**	任务调度，表数据同步

- **szc-server-transfer-parent**

**通讯中心**	应用节点MQTT注册，发消息

- **szc-server-merchant-parent**

**注册中心**	中心接口的注册（每次修改接口的出参、入参和请求方式的时候需要注册）

#### 业务中台

- **szc-server-account-parent**

**账户中心**	用户的账户数据的管理（卡余额、积分、金豆等）

- **szc-server-audit-parent**

**审计中心**	审核资料相关管理

- **szc-server-card-parent**

**卡券中心**	优惠券相关管理，新增、查询、核销等操作

- **szc-server-conf-parent**

**配置中心**	系统配置相关数据的管理

- **szc-server-doc-parent**

**资料中心**	用于文件资料的相关数据管理

- **szc-server-operate-parent**

**运营中心**	主要用于基本信息的数据管理，比如门店、商品、货品等基本数据

- **szc-server-order-parent**

**单据中心**	订单数据的管理

- **szc-server-user-parent**

**用户中心**	用户信息的管理

- **szc-server-pay-parent**

**支付中心**	用于对接第三方支付的中心（目前只对接了建行支付）

- **szc-server-project-parent**

**项目中心**	项目的数据管理，对标与单据中心，目前只用于宜热项目

#### 数据中台

- **szc-server-statistics-parent**

**统计中心**	用于统计订单、库存快照等数据用的（定时任务统计：货品售和出统计、资金统计、库存快照统计、TCAC、要货配货统计）

- **szc-server-data-parent**

**数据中心**	统一数据管理，即使库存快照数据

#### 中台服务

- **szc-server-haoll-all-boot**

**好利来boot工程**	多中心合并后的项目启动boot工程，用于烘培行业的中心合并启动

- **szc-server-yire-all-boot**

**宜热boot工程**	多中心合并后的项目启动boot工程，用于宜热的中心合并启动

- **szc-server-xxx-boot**

**中心独立工程**	各中心独立启动工程，xxx代表各中心缩写

### 企业版应用服务

- **shangzhou-micromall-revision-parent**

**新版微商城**	电商服务中心，包含微商城、第三方订单

- **sza-staff-server-parent**

**员工小程序**	员工小程序的数据服务支持

- **shangzhou-server-micromall-parent**

**老版本微商城**	老版本微商城服务工程 

### 集团版中台服务

- **szgc-server-statistics-parent**

**统计中心** 用于统计分析全集团经营情况

- **szgc-server-share-parent**

**数据共享中心** 用于实现各企业版中台及应用数据共享

- **szgc-server-distribution-parent**

**数据分发中心** 第三方应用接收及数据分发，例如饿了么、美团、有赞等

- **szgc-server-gateway-parent**

**服务网关**  集团版权限认证、中台接口统一入口

## 技术标准

### Java代码规范

Java规范基本可以执行《[阿里巴巴JAVA开发手册]( https://www.w3cschool.cn/alibaba_java/alibaba_java-t9lb3fg7.html)》，开发过程中通过安装插件Alibaba Java Coding Guidelines进行校验。

### 模块命名规范

商周现有代码分组分为，集团版应用、集团版中台、企业版应用、企业版中台，相应的父工程名称为分组缩写+server+xxx+parent，模块内组成参照目录结构，相应的示例如下表：

| 应用分组   | 应用缩写 | 应用父工程示例         |
| ---------- | -------- | ---------------------- |
| 集团版应用 | szga     | szga-server-xxx-parent |
| 集团版中台 | szgc     | szgc-server-xxx-parent |
| 企业版应用 | sza      | sza-server-xxx-parent  |
| 企业版中台 | szc      | szc-server-xxx-parent  |

### 异常编码规范

异常编码分为8位编码和6位编码，8位编码为boot层使用，6位编码为rest层和service层公用。

1. 8编码规则为前两位为应用编码，后6位位rest层错误编码，该编码不可与依赖的rest层编码相同，如两位不够则两位变三位
2. 6位编码前两位为中心编码，后四位为顺序码，如有子模块则第三位为子模块编码，后三位为顺序码

**应用层编码**：约定如下表，应用Rest层另行约定

| 应用名称       | 应用编码 | 编码示例 |
| -------------- | -------- | -------- |
| 企业版中台     | 01       | 01****   |
| 集团版中台     | 02       | 02****   |
| 微商城前台     | 03       | 03****   |
| 微商城后台     | 04       | 04****   |
| 员工小程序前台 | 05       | 05****   |
| 员工小程序后台 | 06       | 06****   |

**中心层编码**：约定如下表

| 中心名称 | 中心编码 | 编码示例                                                     |
| -------- | -------- | ------------------------------------------------------------ |
| 系统框架 | 00       | 00****                                                       |
| 网关中心 | 01       | 01****                                                       |
| 鉴权中心 | 02       | 02****                                                       |
| 配置中心 | 03       | 03****                                                       |
| 审计中心 | 04       | 04****                                                       |
| 运营中心 | 05       | 05****（货品模块：050***、设备模块：051***、机构模块：052***、线下券模块：053***） |
| 单据中心 | 06       | 06****                                                       |
| 支付中心 | 07       | 07****                                                       |
| 通讯中心 | 08       | 08****                                                       |
| 资料中心 | 09       | 09****                                                       |
| 数据中心 | 10       | 10****                                                       |
| 统计中心 | 11       | 11****                                                       |
| 用户中心 | 12       | 12****                                                       |
| 账户中心 | 13       | 13****                                                       |
| 券卡中心 | 14       | 14****                                                       |
| 任务中心 | 15       | 15****                                                       |
| 项目中心 | 16       | 16****                                                       |

### 数据库设计规范

数据库设计规范同样基本遵循《[阿里巴巴JAVA开发手册]( https://www.w3cschool.cn/alibaba_java/alibaba_java-t9lb3fg7.html)》里mysql部分的规范，同时还约定了我们特有的一些规范。

- **表名设计规范**

表名采用26个英文字母（区分大小写）和0-9自然数（一般不需要）加上下划线“_”组成，多个单词用下划线“_”分隔

1. 表名的格式一般为【一级模块+二级模块+三级模块+用途】，模块原则上不超过3个，不强制。用途可不体现，模块也可作为用途使用。不建议使用缩写，尽量使用完整单词，可使用约定俗成的缩写。

常见的用途如下表

| 用途名称 | 用途编码 |用途说明|
| -------- | -------- |-------- |
| 明细     | detail   |主记录明细，表示数据详情，可新增、查询、修改、删除 |
| 日志     | log      |系统操作记录日志，表示一个动作的轨迹，只可以新增，查询，不可用修改、删除 |
| 记录     | record   |业务操作记录，表示带业务属性的业务操作记录，可新增、查询、修改，不可删除|

例如系统访问日志表sys_access_log,用户

常见的缩写如下表

| 缩写名称 | 缩写全拼   | 缩写  |
| -------- | ---------- | ----- |
| 系统     | system     | sys   |
| 账户     | account    | acct  |
| 属性     | attributes | attr  |
| 支付     | payment    | pay   |
| 参数     | parameter  | param |

2. 模块与模块之间的关联表则需判断已那张表为主，然后根据那张表为蓝本后面接第二张表的最后一个模块名拼接，如第二个模块有用途属性，则统一拼接在一起。例如用户表sys_user，角色表sys_role，用户角色表sys_user_role.
3. 全部小写命令，禁止出现大写
4. 表的名称一般使用名词或者动宾短语
5. 表的名字不能取太长，一般不超过三个英文单词，不推荐中文拼音

- **字段设计规范**

1. 字段类型能用数字型尽量使用数字型，长度能段尽量使用更短的字段
2. 表示判断类的字段必须添加is_前缀，字段类型必须使用bit或者tinyint(1),（1 表示是，0 表示否）
3. 类型类字段尽量使用tinyint(4),如实在需要适当扩充字段长度
4. 其他表的外键需要携带外键表的最后一个模块+id进行拼接，如有用途则拼接在模块后面
5. 表必备三字段：id，create_time，update_time。其中 id 必为主键，类型为 bigint、单表时自增、步长为 1，只有在数据和并发足够大的情况下，或者数据有分库分表场景的情况下才采用分布式主键。create_time，update_time，opt_time 的类型均为datetime 类型，如果要记录时区信息，那么类型设置为 timestamp。其中create_time和update_time设置默认值now(),update_time同时需要设置修改更新。
6. 表如需频繁根据日期查询，需要设置date类型字段


