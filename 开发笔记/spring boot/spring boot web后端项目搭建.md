# spring boot web 项目搭建

**存放思路：**

> 先在本地搭建一个初始化的工程，然后将该项目关联到GitHub。

**包含内容**

*整合*

> 1. 整合 mybatis。为操作数据库提供 半orm 支持。
> 2. 整合 mybatis 通用 mapper。为操作数据库提供 orm 支持。
> 3. 整合 uid 生成器。为全局唯一 id 的生成提供支持。
> 4. 整合 minio 文件服务。包含工具类。

*配置*

> 1. 配置 返回类。
> 2. 配置 异常 统一管理。
> 3. 配置 日志。
> 4. 配置 跨域

## 1. idea 新建一个空的 spring boot 工程

**步骤：**

> File - New - Project - 选择 Spring Initializr - 选择jdk - 点击Next - 填写和选择工程相关信息 - 选择 Lombok、web 依赖- 点击 Next Finish 到结束

## 2. 导包 .pom 文件

**由于本项目是一个maven项目，故导包只需要编辑 .pom 文件就好**

*tips: 为了能更快速的下载 jar 包，建议先相关的 maven 配置*

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- mybatis -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
        <!-- mybatis 通用 mapper -->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
            <version>2.1.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.22</version>
        </dependency>
        <!-- uid -->
        <dependency>
            <groupId>com.github.wujun234</groupId>
            <artifactId>uid-generator-spring-boot-starter</artifactId>
            <version>1.0.2.RELEASE</version>
        </dependency>
        <!--minio文件系统-->
        <dependency>
            <groupId>io.minio</groupId>
            <artifactId>minio</artifactId>
            <version>7.1.0</version>
        </dependency>
        <!-- JSON解析 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.51</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
    </dependencies>
```



## 3.  配置

### 3.1 目录结构搭建

*框架骨架*

1. controller。服务入口
2. service。业务逻辑处理
3. dao。数据库入口
4. entity。数据表映射
5. dto。传输对象

*辅助*

1. utils。工具类
2. config。配置类
3. enums。枚举类
4. exception。异常类

*资源*

1. common。共有资源
2. resource 下 mapper。xml sql资源



### 3.2 配置 application.yaml 文件

一般维护两个 .yaml 文件，-dev.yaml 是开发环境下的，这个一般是开发人员自己的，不用push到远程，-st.yaml是标准环境下的，所有开发人员公用。

1. application.yaml 文件

    ```yaml
    spring:
      profiles:
        active: dev
    ```

2. application-dev.yaml 与application-st.yaml 暂时一样（暂时这么多，后面配置回继续加，最终版在最后给出）

   ```yaml
   # server
   server:
    port: 8080
   # datasource
   spring:
     datasource:
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://IP:端口/数据库名?serverTimezone=UTC
       username: xxxx
       password: xxxx
   # mybatis
   mybatis:
     type-aliases-package: com.n303b.gsb_data.entity # entity
     mapper-locations: mapper/*.xml # xml mapper
     configuration: # other
       map-underscore-to-camel-case: true
   ```
   

	***注意： yaml 文件不允许中文注册，否则项目启动失败，报错如下***

   ```console
   15:02:55.146 [main] DEBUG org.springframework.boot.diagnostics.FailureAnalyzers - FailureAnalyzer org.springframework.boot.diagnostics.analyzer.ValidationExceptionFailureAnalyzer@687e99d8 failed
   java.lang.NoClassDefFoundError: javax/validation/ValidationException
   	at org.springframework.boot.diagnostics.analyzer.ValidationExceptionFailureAnalyzer.analyze(ValidationExceptionFailureAnalyzer.java:31)
   	at org.springframework.boot.diagnostics.AbstractFailureAnalyzer.analyze(AbstractFailureAnalyzer.java:35)
   	at org.springframework.boot.diagnostics.FailureAnalyzers.analyze(FailureAnalyzers.java:118)
   	at org.springframework.boot.diagnostics.FailureAnalyzers.reportException(FailureAnalyzers.java:111)
   	at org.springframework.boot.SpringApplication.reportFailure(SpringApplication.java:846)
   	at org.springframework.boot.SpringApplication.handleRunFailure(SpringApplication.java:821)
   	at org.springframework.boot.SpringApplication.run(SpringApplication.java:336)
   	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1309)
   	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1298)
   	at com.n303b.gsb_data.GsbDataApplication.main(GsbDataApplication.java:12)
   Caused by: java.lang.ClassNotFoundException: javax.validation.ValidationException
   	at java.net.URLClassLoader.findClass(URLClassLoader.java:382)
   	at java.lang.ClassLoader.loadClass(ClassLoader.java:418)
   	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:355)
   	at java.lang.ClassLoader.loadClass(ClassLoader.java:351)
   	... 10 common frames omitted
   15:02:55.148 [main] DEBUG org.springframework.boot.diagnostics.FailureAnalyzers - FailureAnalyzer org.springframework.boot.liquibase.LiquibaseChangelogMissingFailureAnalyzer@1ebd319f failed
   java.lang.NoClassDefFoundError: liquibase/exception/ChangeLogParseException
   	at org.springframework.boot.liquibase.LiquibaseChangelogMissingFailureAnalyzer.analyze(LiquibaseChangelogMissingFailureAnalyzer.java:33)
   	at org.springframework.boot.diagnostics.AbstractFailureAnalyzer.analyze(AbstractFailureAnalyzer.java:35)
   	at org.springframework.boot.diagnostics.FailureAnalyzers.analyze(FailureAnalyzers.java:118)
   	at org.springframework.boot.diagnostics.FailureAnalyzers.reportException(FailureAnalyzers.java:111)
   	at org.springframework.boot.SpringApplication.reportFailure(SpringApplication.java:846)
   	at org.springframework.boot.SpringApplication.handleRunFailure(SpringApplication.java:821)
   	at org.springframework.boot.SpringApplication.run(SpringApplication.java:336)
   	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1309)
   	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1298)
   	at com.n303b.gsb_data.GsbDataApplication.main(GsbDataApplication.java:12)
   Caused by: java.lang.ClassNotFoundException: liquibase.exception.ChangeLogParseException
   	at java.net.URLClassLoader.findClass(URLClassLoader.java:382)
   	at java.lang.ClassLoader.loadClass(ClassLoader.java:418)
   	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:355)
   	at java.lang.ClassLoader.loadClass(ClassLoader.java:351)
   	... 10 common frames omitted
   15:02:55.150 [main] ERROR org.springframework.boot.SpringApplication - Application run failed
   org.yaml.snakeyaml.error.YAMLException: java.nio.charset.MalformedInputException: Input length = 1
   	at org.yaml.snakeyaml.reader.StreamReader.update(StreamReader.java:218)
   	at org.yaml.snakeyaml.reader.StreamReader.ensureEnoughData(StreamReader.java:176)
   	at org.yaml.snakeyaml.reader.StreamReader.ensureEnoughData(StreamReader.java:171)
   	at org.yaml.snakeyaml.reader.StreamReader.peek(StreamReader.java:126)
   	at org.yaml.snakeyaml.scanner.ScannerImpl.scanToNextToken(ScannerImpl.java:1177)
   	at org.yaml.snakeyaml.scanner.ScannerImpl.fetchMoreTokens(ScannerImpl.java:287)
   	at org.yaml.snakeyaml.scanner.ScannerImpl.checkToken(ScannerImpl.java:227)
   	at org.yaml.snakeyaml.parser.ParserImpl$ParseImplicitDocumentStart.produce(ParserImpl.java:195)
   	at org.yaml.snakeyaml.parser.ParserImpl.peekEvent(ParserImpl.java:158)
   	at org.yaml.snakeyaml.parser.ParserImpl.checkEvent(ParserImpl.java:148)
   	at org.yaml.snakeyaml.composer.Composer.checkNode(Composer.java:82)
   	at org.yaml.snakeyaml.constructor.BaseConstructor.checkData(BaseConstructor.java:123)
   	at org.yaml.snakeyaml.Yaml$1.hasNext(Yaml.java:507)
   	at org.springframework.beans.factory.config.YamlProcessor.process(YamlProcessor.java:200)
   	at org.springframework.beans.factory.config.YamlProcessor.process(YamlProcessor.java:164)
   	at org.springframework.boot.env.OriginTrackedYamlLoader.load(OriginTrackedYamlLoader.java:84)
   	at org.springframework.boot.env.YamlPropertySourceLoader.load(YamlPropertySourceLoader.java:50)
   	at org.springframework.boot.context.config.StandardConfigDataLoader.load(StandardConfigDataLoader.java:45)
   	at org.springframework.boot.context.config.StandardConfigDataLoader.load(StandardConfigDataLoader.java:34)
   	at org.springframework.boot.context.config.ConfigDataLoaders.load(ConfigDataLoaders.java:102)
   	at org.springframework.boot.context.config.ConfigDataImporter.load(ConfigDataImporter.java:118)
   	at org.springframework.boot.context.config.ConfigDataImporter.resolveAndLoad(ConfigDataImporter.java:82)
   	at org.springframework.boot.context.config.ConfigDataEnvironmentContributors.withProcessedImports(ConfigDataEnvironmentContributors.java:118)
   	at org.springframework.boot.context.config.ConfigDataEnvironment.processWithProfiles(ConfigDataEnvironment.java:294)
   	at org.springframework.boot.context.config.ConfigDataEnvironment.processAndApply(ConfigDataEnvironment.java:223)
   	at org.springframework.boot.context.config.ConfigDataEnvironmentPostProcessor.postProcessEnvironment(ConfigDataEnvironmentPostProcessor.java:88)
   	at org.springframework.boot.context.config.ConfigDataEnvironmentPostProcessor.postProcessEnvironment(ConfigDataEnvironmentPostProcessor.java:80)
   	at org.springframework.boot.env.EnvironmentPostProcessorApplicationListener.onApplicationEnvironmentPreparedEvent(EnvironmentPostProcessorApplicationListener.java:100)
   	at org.springframework.boot.env.EnvironmentPostProcessorApplicationListener.onApplicationEvent(EnvironmentPostProcessorApplicationListener.java:86)
   	at org.springframework.context.event.SimpleApplicationEventMulticaster.doInvokeListener(SimpleApplicationEventMulticaster.java:203)
   	at org.springframework.context.event.SimpleApplicationEventMulticaster.invokeListener(SimpleApplicationEventMulticaster.java:196)
   	at org.springframework.context.event.SimpleApplicationEventMulticaster.multicastEvent(SimpleApplicationEventMulticaster.java:170)
   	at org.springframework.context.event.SimpleApplicationEventMulticaster.multicastEvent(SimpleApplicationEventMulticaster.java:148)
   	at org.springframework.boot.context.event.EventPublishingRunListener.environmentPrepared(EventPublishingRunListener.java:82)
   	at org.springframework.boot.SpringApplicationRunListeners.lambda$environmentPrepared$2(SpringApplicationRunListeners.java:63)
   	at java.util.ArrayList.forEach(ArrayList.java:1259)
   	at org.springframework.boot.SpringApplicationRunListeners.doWithListeners(SpringApplicationRunListeners.java:117)
   	at org.springframework.boot.SpringApplicationRunListeners.doWithListeners(SpringApplicationRunListeners.java:111)
   	at org.springframework.boot.SpringApplicationRunListeners.environmentPrepared(SpringApplicationRunListeners.java:62)
   	at org.springframework.boot.SpringApplication.prepareEnvironment(SpringApplication.java:362)
   	at org.springframework.boot.SpringApplication.run(SpringApplication.java:320)
   	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1309)
   	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1298)
   	at com.n303b.gsb_data.GsbDataApplication.main(GsbDataApplication.java:12)
   Caused by: java.nio.charset.MalformedInputException: Input length = 1
   	at java.nio.charset.CoderResult.throwException(CoderResult.java:281)
   	at sun.nio.cs.StreamDecoder.implRead(StreamDecoder.java:339)
   	at sun.nio.cs.StreamDecoder.read(StreamDecoder.java:178)
   	at java.io.InputStreamReader.read(InputStreamReader.java:184)
   	at org.yaml.snakeyaml.reader.UnicodeReader.read(UnicodeReader.java:125)
   	at org.yaml.snakeyaml.reader.StreamReader.update(StreamReader.java:183)
   	... 43 common frames omitted
   
   Process finished with exit code 1
   ```

### 3.3 配置 通用 mapper

1. dao 目录下新建 base 包，并创建 `BaseDao`

   ```java
   package com.n303b.gsb_data.dao.base;
   
   import tk.mybatis.mapper.common.Mapper;
   import tk.mybatis.mapper.common.MySqlMapper;
   
   public interface BaseDao<T> extends Mapper<T>, MySqlMapper<T> {
   }
   ```

2. dao 目录下创建 mapper，用来保存  mapper 类

3. yaml 文件添加配置

   ```yaml
   # mapper
   mapper:
     mappers: com.n303b.gsb_data.dao.base.BaseDao # base mapper
     identity: MYSQL
   ```

4. 启动类添加 mapper 扫描包注解

   ```java
   package com.n303b.gsb_data;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import tk.mybatis.spring.annotation.MapperScan;
   
   @MapperScan("com.n303b.gsb_data.dao.mapper") // 添加 mapper 扫描包
   @SpringBootApplication
   public class GsbDataApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(GsbDataApplication.class, args);
       }
   
   }
   ```

5. 为了消除警告 "No MyBatis mapper was found in..."，在 创建一个类用 `@Mapper` 注解标注

   ```java
   package com.n303b.gsb_data.dao.base;
   
   import org.apache.ibatis.annotations.Mapper;
   
   // 为了消除mapper扫描警告
   @Mapper
   public interface RemoveWarning {
   }
   ```

   

### 3.4 配置 日志

1. 在 resources 目录下创建 `logback-spring.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 日志级别从低到高分为TRACE < DEBUG < INFO < WARN < ERROR < FATAL，如果设置为WARN，则低于WARN的信息都不会输出 -->
<!-- scan:当此属性设置为true时，配置文档如果发生改变，将会被重新加载，默认值为true -->
<!-- scanPeriod:设置监测配置文档是否有修改的时间间隔，如果没有给出时间单位，默认单位是毫秒。
                 当scan为true时，此属性生效。默认的时间间隔为1分钟。 -->
<!-- debug:当此属性设置为true时，将打印出logback内部日志信息，实时查看logback运行状态。默认值为false。 -->
<configuration  scan="true" scanPeriod="10 seconds">
    <contextName>logback</contextName>

    <!-- name的值是变量的名称，value的值时变量定义的值。通过定义的值会被插入到logger上下文中。定义后，可以使“${}”来使用变量。 -->
    <property name="log.path" value="${log.path}/web_info.log" />

    <!--0. 日志格式和颜色渲染 -->
    <!-- 彩色日志依赖的渲染类 -->
    <conversionRule conversionWord="clr" converterClass="org.springframework.boot.logging.logback.ColorConverter" />
    <conversionRule conversionWord="wex" converterClass="org.springframework.boot.logging.logback.WhitespaceThrowableProxyConverter" />
    <conversionRule conversionWord="wEx" converterClass="org.springframework.boot.logging.logback.ExtendedWhitespaceThrowableProxyConverter" />
    <!-- 彩色日志格式 -->
    <property name="CONSOLE_LOG_PATTERN" value="${CONSOLE_LOG_PATTERN:-%clr(%d{yyyy-MM-dd HH:mm:ss.SSS}){faint} %clr(${LOG_LEVEL_PATTERN:-%5p}) %clr(${PID:- }){magenta} %clr(---){faint} %clr([%15.15t]){faint} %clr(%-40.40logger{39}){cyan} %clr(:){faint} %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}}"/>

    <!--1. 输出到控制台-->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <!--此日志appender是为开发使用，只配置最底级别，控制台输出的日志级别是大于或等于此级别的日志信息-->
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>debug</level>
        </filter>
        <encoder>
            <Pattern>${CONSOLE_LOG_PATTERN}</Pattern>
            <!-- 设置字符集 -->
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <!--2. 输出到文档-->
    <!-- 2.1 level为 DEBUG 日志，时间滚动输出  -->
    <appender name="DEBUG_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 正在记录的日志文档的路径及文档名 -->
        <file>${log.path}/web_debug.log</file>
        <!--日志文档输出格式-->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            <charset>UTF-8</charset> <!-- 设置字符集 -->
        </encoder>
        <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 日志归档 -->
            <fileNamePattern>${log.path}/web-debug-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <!--日志文档保留天数-->
            <maxHistory>15</maxHistory>
        </rollingPolicy>
        <!-- 此日志文档只记录debug级别的 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>debug</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- 2.2 level为 INFO 日志，时间滚动输出  -->
    <appender name="INFO_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 正在记录的日志文档的路径及文档名 -->
        <file>${log.path}/web_info.log</file>
        <!--日志文档输出格式-->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
        <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 每天日志归档路径以及格式 -->
            <fileNamePattern>${log.path}/web-info-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <!--日志文档保留天数-->
            <maxHistory>15</maxHistory>
        </rollingPolicy>
        <!-- 此日志文档只记录info级别的 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>info</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- 2.3 level为 WARN 日志，时间滚动输出  -->
    <appender name="WARN_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 正在记录的日志文档的路径及文档名 -->
        <file>${log.path}/web_warn.log</file>
        <!--日志文档输出格式-->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            <charset>UTF-8</charset> <!-- 此处设置字符集 -->
        </encoder>
        <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${log.path}/web-warn-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <!--日志文档保留天数-->
            <maxHistory>15</maxHistory>
        </rollingPolicy>
        <!-- 此日志文档只记录warn级别的 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>warn</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- 2.4 level为 ERROR 日志，时间滚动输出  -->
    <appender name="ERROR_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 正在记录的日志文档的路径及文档名 -->
        <file>${log.path}/web_error.log</file>
        <!--日志文档输出格式-->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            <charset>UTF-8</charset> <!-- 此处设置字符集 -->
        </encoder>
        <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${log.path}/web-error-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <!--日志文档保留天数-->
            <maxHistory>15</maxHistory>
        </rollingPolicy>
        <!-- 此日志文档只记录ERROR级别的 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>ERROR</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!--
        <logger>用来设置某一个包或者具体的某一个类的日志打印级别、
        以及指定<appender>。<logger>仅有一个name属性，
        一个可选的level和一个可选的addtivity属性。
        name:用来指定受此logger约束的某一个包或者具体的某一个类。
        level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF，
              还有一个特俗值INHERITED或者同义词NULL，代表强制执行上级的级别。
              如果未设置此属性，那么当前logger将会继承上级的级别。
        addtivity:是否向上级logger传递打印信息。默认是true。
        <logger name="org.springframework.web" level="info"/>
        <logger name="org.springframework.scheduling.annotation.ScheduledAnnotationBeanPostProcessor" level="INFO"/>
    -->

    <!--
        使用mybatis的时候，sql语句是debug下才会打印，而这里我们只配置了info，所以想要查看sql语句的话，有以下两种操作：
        第一种把<root level="info">改成<root level="DEBUG">这样就会打印sql，不过这样日志那边会出现很多其他消息
        第二种就是单独给dao下目录配置debug模式，代码如下，这样配置sql语句会打印，其他还是正常info级别：
        【logging.level.org.mybatis=debug logging.level.dao=debug】
     -->

    <!--
        root节点是必选节点，用来指定最基础的日志输出级别，只有一个level属性
        level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF，
        不能设置为INHERITED或者同义词NULL。默认是DEBUG
        可以包含零个或多个元素，标识这个appender将会添加到这个logger。
    -->

    <!-- 4. 最终的策略 -->
    <!-- 4.1 开发环境:打印控制台-->
    <springProfile name="dev">
        <logger name="com.reason.gsny" level="debug"/>
    </springProfile>

    <root level="info">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="DEBUG_FILE" />
        <appender-ref ref="INFO_FILE" />
        <appender-ref ref="WARN_FILE" />
        <appender-ref ref="ERROR_FILE" />
    </root>

    <!-- 4.2 生产环境:输出到文档
    <springProfile name="pro">
        <root level="info">
            <appender-ref ref="CONSOLE" />
            <appender-ref ref="DEBUG_FILE" />
            <appender-ref ref="INFO_FILE" />
            <appender-ref ref="ERROR_FILE" />
            <appender-ref ref="WARN_FILE" />
        </root>
    </springProfile> -->

</configuration>
```

2. yaml 文件添加日志位置配置

   ``` yaml
   # log
   logging:
     config: classpath:logback-spring.xml
   ```
   
3. 使用示例

   ```java
   // 在A类中引入
   public static Logger logger = LoggerFactory.getLogger(A.class);
   // 使用
   logger.info("msg");
   ```

4. 添加日志文档目录到 .gitignore，日志一般不用 git 去维护

   ```
   
   ```



### 3.5 配置 统一返回类R

1. enums  目录下创建返回类型枚举 `ResultCodeEnum`

   ```java
   package com.n303b.gsb_data.enums;
   
   import lombok.Getter;
   
   @Getter
   public enum ResultCodeEnum {
   
       SUCCESS(true,2000,"成功"),
       UNKNOWN_ERROR(false,2001,"未知错误"),
       PARAM_ERROR(false,2002,"参数错误"),
       NULL_POINT(false,2003,"空指针异常"),
       HTTP_CLIENT_ERROR(false,2004,"客户端连接异常");
       /**
        * 响应是否成功
        */
       private Boolean success;
       /**
        * 响应状态码
        */
       private Integer code;
       /**
        * 响应信息
        */
       private String message;
   
       ResultCodeEnum(boolean success, Integer code, String message) {
           this.success = success;
           this.code = code;
           this.message = message;
       }
   }
   ```

2. utils 目录下创建 返回类 `R`

   ```java
   package com.n303b.gsb_data.utils;
   
   import com.n303b.gsb_data.enums.ResultCodeEnum;
   import lombok.Data;
   
   import java.util.HashMap;
   import java.util.Map;
   
   /**
    * 默认：
    * 1、无数据成功
    * 2、无数据失败
    * 3、无数据其他
    *
    * 自定义：
    * 链式编程，可设置 是否成功？状态码？信息？数据？
    */
   @Data
   public class R {
       private Boolean success;
   
       private Integer code;
   
       private String message;
   
       private Map<String, Object> data = new HashMap<>();
   
       /**
        * 构造器私有
        */
       private R() {
       }
   
       /**
        * 通用返回成功
        */
       public static R ok() {
           R r = new R();
           r.setSuccess(ResultCodeEnum.SUCCESS.getSuccess());
           r.setCode(ResultCodeEnum.SUCCESS.getCode());
           r.setMessage(ResultCodeEnum.SUCCESS.getMessage());
           return r;
       }
   
       /**
        * 通用返回失败，未知错误
        *
        * @return
        */
       public static R error() {
           R r = new R();
           r.setSuccess(ResultCodeEnum.UNKNOWN_ERROR.getSuccess());
           r.setCode(ResultCodeEnum.UNKNOWN_ERROR.getCode());
           r.setMessage(ResultCodeEnum.UNKNOWN_ERROR.getMessage());
           return r;
       }
   
       /**
        * 设置结果，形参为结果枚举
        *
        * @param result
        * @return
        */
       public static R setResult(ResultCodeEnum result) {
           R r = new R();
           r.setSuccess(result.getSuccess());
           r.setCode(result.getCode());
           r.setMessage(result.getMessage());
           return r;
       }
   
       /**
        * ------------使用链式编程，返回类本身----------
        * 自定义返回数据
        *
        * @param map
        * @return
        */
       public R data(Map<String, Object> map) {
           this.setData(map);
           return this;
       }
   
       /**
        * 通用设置data
        *
        * @param key
        * @param value
        * @return
        */
       public R data(String key, Object value) {
           this.data.put(key, value);
           return this;
       }
   
       /**
        * 自定义状态信息
        *
        * @param message
        * @return
        */
       public R message(String message) {
           this.setMessage(message);
           return this;
       }
   
       /**
        * 自定义状态码
        *
        * @param code
        * @return
        */
       public R code(Integer code) {
           this.setCode(code);
           return this;
       }
   
       /**
        * 自定义返回结果
        *
        * @param success
        * @return
        */
       public R success(Boolean success) {
           this.setSuccess(success);
           return this;
       }
   }
   
   ```

3. 使用

   ```java
   @GetMapping("useR")
   public R useR() {
       return R.ok();
   }
   ```



### 3.6 配置 异常

1. utils 目录下创建 `ExceptionUtils` 打印异常的工具类

   ```java
   package com.n303b.gsb_data.utils;
   
   import lombok.extern.slf4j.Slf4j;
   
   import java.io.IOException;
   import java.io.PrintWriter;
   import java.io.StringWriter;
   
   /**
    * 日志处理工具类
    */
   @Slf4j
   public class ExceptionUtils {
   
       /**
        * 打印异常信息
        */
       public static String getMessage(Exception e) {
           String swStr = null;
           try (
                   StringWriter sw = new StringWriter();
                   PrintWriter pw = new PrintWriter(sw)) {
               e.printStackTrace(pw);
               pw.flush();
               sw.flush();
               swStr = sw.toString();
           } catch (IOException ex) {
               ex.printStackTrace();
               log.error(ex.getMessage());
           }
           return swStr;
       }
   }
   ```

2. exception 目录下创建 **自定义异常** 和  **全局异常**

   自定义异常

   ```java
   package com.n303b.gsb_data.exception;
   
   import com.n303b.gsb_data.enums.ResultCodeEnum;
   import lombok.Data;
   
   @Data
   public class CMSException extends RuntimeException {
       private Integer code;
   
       public CMSException(Integer code, String message) {
           super(message);
           this.code = code;
       }
   
       public CMSException(ResultCodeEnum resultCodeEnum) {
           super(resultCodeEnum.getMessage());
           this.code = resultCodeEnum.getCode();
       }
   
       @Override
       public String toString() {
           return "CMSException{" + "code=" + code + ", message=" + this.getMessage() + '}';
       }
   }
   ```
   全局异常

      ```java
   package com.n303b.gsb_data.exception;
   
   import com.n303b.gsb_data.enums.ResultCodeEnum;
   import com.n303b.gsb_data.utils.ExceptionUtils;
   import com.n303b.gsb_data.utils.R;
   import lombok.extern.slf4j.Slf4j;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.web.bind.annotation.ControllerAdvice;
   import org.springframework.web.bind.annotation.ExceptionHandler;
   import org.springframework.web.bind.annotation.ResponseBody;
   import org.springframework.web.client.HttpClientErrorException;
   
   @ControllerAdvice
   @Slf4j
   public class GlobalExceptionHandler {
   
       public static Logger looger = LoggerFactory.getLogger(GlobalExceptionHandler.class);
   
       /**-------- 通用异常处理方法 --------**/
       @ExceptionHandler(Exception.class)
       @ResponseBody
       public R error(Exception e) {
           // 通用异常结果打印到日志
           log.error(ExceptionUtils.getMessage(e));
           return R.error();
       }
   
       /**-------- 指定异常处理方法 --------**/
       @ExceptionHandler(NullPointerException.class)
       @ResponseBody
       public R error(NullPointerException e) {
           e.printStackTrace();
           return R.setResult(ResultCodeEnum.NULL_POINT);
       }
   
       @ExceptionHandler(HttpClientErrorException.class)
       @ResponseBody
       public R error(IndexOutOfBoundsException e) {
           e.printStackTrace();
           return R.setResult(ResultCodeEnum.HTTP_CLIENT_ERROR);
       }
   
       /**-------- 自定义定异常处理方法 --------**/
       @ExceptionHandler(CMSException.class)
       @ResponseBody
       public R error(CMSException e) {
           looger.warn(e.getMessage());
           return R.error().message(e.getMessage()).code(e.getCode());
       }
   }
      ```




### 3.7 配置 跨域

在 config 目录下创建 `CorsConfig`

```java
package com.n303b.gsb_data.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;

@Configuration
public class CorsConfig {

    private CorsConfiguration addCorsConfig() {
        CorsConfiguration corsConfiguration = new CorsConfiguration();
        corsConfiguration.addAllowedOriginPattern("*");
        corsConfiguration.addAllowedHeader("*");
        corsConfiguration.addAllowedMethod("*");
        corsConfiguration.setAllowCredentials(true);

        return corsConfiguration;
    }

    @Bean
    public CorsFilter corsFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", addCorsConfig());
        return new CorsFilter(source);
    }
}
```



### 3.8 配置 minio

1. yaml 文件配置相关资源。

   ```yaml
   # minio
   minio:
     url: http://127.0.0.1:9000
     accessKey: minioadmin
     secretKey: minioadmin
   ```

2. config 目录创建 minio 配置类 `MinioConfig`，实现项目启动后完成 minio bean 单例注册。

   ```java
   package com.n303b.gsb_data.config;
   
   import io.minio.MinioClient;
   import lombok.Data;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   @Data
   @Configuration
   public class MinioConfig {
   
       @Value("${minio.url}")
       String minioUrl;
   
       @Value("${minio.accessKey}")
       String accessKey;
   
       @Value("${minio.secretKey}")
       String secretKey;
   
       @Bean
       public MinioClient generateMinioClient() {
   
           return MinioClient.builder()
                   .endpoint(minioUrl)
                   .credentials(accessKey, secretKey)
                   .build();
   
       }
   }
   ```

3. minio 工具类 `MinioUtils`

   ```java
   package com.n303b.gsb_data.utils;
   
   import com.alibaba.fastjson.JSONObject;
   import com.n303b.gsb_data.config.MinioConfig;
   import io.minio.*;
   import io.minio.http.Method;
   import io.minio.messages.Bucket;
   import io.minio.messages.Item;
   import lombok.SneakyThrows;
   import org.springframework.stereotype.Component;
   import org.springframework.web.multipart.MultipartFile;
   
   import javax.annotation.Resource;
   import java.io.InputStream;
   import java.util.ArrayList;
   import java.util.List;
   import java.util.Optional;
   
   @Component
   public class MinioUtils {
   
       @Resource
       private MinioClient client;
       @Resource
       private MinioConfig minioConfig;
   
       /**
        * 创建bucket
        *
        * @param bucketName bucket名称
        */
       @SneakyThrows
       public void createBucket(String bucketName){
           if (!client.bucketExists(BucketExistsArgs.builder().bucket(bucketName).build())) {
               client.makeBucket(MakeBucketArgs.builder().bucket(bucketName).build());
           }
       }
   
       /**
        * 获取存储桶策略
        *
        * @param bucketName 存储桶名称
        * @return json
        */
       @SneakyThrows
       private JSONObject getBucketPolicy(String bucketName){
           String bucketPolicy = client
                   .getBucketPolicy(GetBucketPolicyArgs.builder().bucket(bucketName).build());
           return JSONObject.parseObject(bucketPolicy);
       }
       /**
        * 获取全部bucket
        *
        */
       @SneakyThrows
       public List<Bucket> getAllBuckets(){
           return client.listBuckets();
       }
   
       /**
        * 根据bucketName获取信息
        *
        * @param bucketName bucket名称
        */
       @SneakyThrows
       public Optional<Bucket> getBucket(String bucketName) {
           return client.listBuckets().stream().filter(b -> b.name().equals(bucketName)).findFirst();
       }
       /**
        * 根据bucketName删除信息
        *
        * @param bucketName bucket名称
        */
       @SneakyThrows
       public void removeBucket(String bucketName){
           client.removeBucket(RemoveBucketArgs.builder().bucket(bucketName).build());
       }
       /**
        * 判断文件是否存在
        *
        * @param bucketName 存储桶
        * @param objectName 对象
        * @return true：存在
        */
       public boolean doesObjectExist(String bucketName, String objectName) {
           boolean exist = true;
           try {
               client.statObject(StatObjectArgs.builder().bucket(bucketName).object(objectName).build());
           } catch (Exception e) {
               exist = false;
           }
           return exist;
       }
       /**
        * 判断文件夹是否存在
        *
        * @param bucketName 存储桶
        * @param objectName 文件夹名称（去掉/）
        * @return true：存在
        */
       public boolean doesFolderExist(String bucketName, String objectName) {
           boolean exist = false;
           try {
               Iterable<Result<Item>> results = client.listObjects(
                       ListObjectsArgs.builder().bucket(bucketName).prefix(objectName).recursive(false).build());
               for (Result<Item> result : results) {
                   Item item = result.get();
                   if (item.isDir() && objectName.equals(item.objectName())) {
                       exist = true;
                   }
               }
           } catch (Exception e) {
               exist = false;
           }
           return exist;
       }
   
       /**
        * 根据文件前置查询文件
        *
        * @param bucketName bucket名称
        * @param prefix 前缀
        * @param recursive 是否递归查询
        * @return MinioItem 列表
        */
       @SneakyThrows
       public List<Item> getAllObjectsByPrefix(String bucketName, String prefix,
                                               boolean recursive){
           List<Item> list = new ArrayList<>();
           Iterable<Result<Item>> objectsIterator = client.listObjects(
                   ListObjectsArgs.builder().bucket(bucketName).prefix(prefix).recursive(recursive).build());
           if (objectsIterator != null) {
               for (Result<Item> o : objectsIterator) {
                   Item item = o.get();
                   list.add(item);
               }
           }
           return list;
       }
       /**
        * 获取文件流
        *
        * @param bucketName bucket名称
        * @param objectName 文件名称
        * @return 二进制流
        */
       @SneakyThrows
       public InputStream getObject(String bucketName, String objectName){
           return client.getObject(GetObjectArgs.builder().bucket(bucketName).object(objectName).build());
       }
       /**
        * 通过MultipartFile，上传文件
        *
        * @param bucketName 存储桶
        * @param file 文件
        * @param objectName 对象名
        */
       @SneakyThrows
       public String putObject(String bucketName, MultipartFile file,
                               String objectName, String contentType){
           // 判断存储桶是否存在
           createBucket(bucketName);
           client.putObject(PutObjectArgs.builder()
                   .bucket(bucketName)
                   .object(objectName)
                   .stream(file.getInputStream(), file.getSize(), -1)
                   .contentType(contentType)
                   .build());
           return '/' + bucketName + '/' + objectName;
       }
   
       /**
        * 获取文件外链
        *
        * @param bucketName bucket名称
        * @param objectName 文件名称
        * @param expires 过期时间 <=7 秒级
        * @return url
        */
       @SneakyThrows
       public String getPresignedObjectUrl(String bucketName, String objectName,
                                           Integer expires) {
           return client.getPresignedObjectUrl(GetPresignedObjectUrlArgs.builder()
                   .method(Method.GET)
                   .bucket(bucketName)
                   .object(objectName)
                   .expiry(expires)
                   .build());
       }
   
   }
   ```

### 3.9 配置 uid

前面 pom 文件已经导入 spring boot uid 的 starter，但还差一步：**数据库创建一张表 WORKER_NODE**

```sql
create table WORKER_NODE
(
    ID          int auto_increment comment 'auto increment id'
        primary key,
    HOST_NAME   varchar(64) not null comment 'host name',
    PORT        varchar(64) not null comment 'port',
    TYPE        int         not null comment 'node type: CONTAINER(1), ACTUAL(2), FAKE(3)',
    LAUNCH_DATE date        not null comment 'launch date',
    MODIFIED    timestamp   not null comment 'modified time',
    CREATED     timestamp   not null comment 'created time'
)
    comment 'DB WorkerID Assigner for UID Generator';
```

*tips：表名大写，否则有奇怪的错误*

### 3.10 配置 其他

1. 数据工具类

   ```java
   package com.n303b.gsb_data.utils;
   
   import org.springframework.beans.BeanUtils;
   import org.springframework.beans.BeanWrapper;
   import org.springframework.beans.BeanWrapperImpl;
   
   import java.text.SimpleDateFormat;
   import java.util.Date;
   import java.util.HashSet;
   import java.util.Set;
   
   public class DataUtils {
   
       public static String getSysTimeByFormat(String format) {
           if (null == format) {
               return new SimpleDateFormat("yyyyMMddHHmmss").format(new Date());//默认格式
           } else {
               return new SimpleDateFormat(format).format(new Date());
           }
       }
   
       public static String getSysTimeDefaultFormat() {
           return new SimpleDateFormat("yyyyMMddHHmmss").format(new Date());//默认格式
       }
   
       public static String timeFormat(String args) {
           StringBuilder stringBuilder = new StringBuilder(args);
           if (args.length() == 6) {
               stringBuilder.insert(2,":");
               stringBuilder.insert(5,":");
           } else if (args.length() == 8){
               stringBuilder.insert(4,"-");
               stringBuilder.insert(7,"-");
           }
           else if(args.length() == 14) {
               stringBuilder.insert(4,"-");
               stringBuilder.insert(7,"-");
               stringBuilder.insert(10," ");
               stringBuilder.insert(13,":");
               stringBuilder.insert(16,":");
           }
           return stringBuilder.toString();
       }
   
       public static String[] getNullPropertyNames(Object source) {
           final BeanWrapper src = new BeanWrapperImpl(source);
           java.beans.PropertyDescriptor[] pds = src.getPropertyDescriptors();
   
           Set<String> emptyNames = new HashSet<String>();
           for (java.beans.PropertyDescriptor pd : pds) {
               Object srcValue = src.getPropertyValue(pd.getName());
               if (srcValue == null) emptyNames.add(pd.getName());
           }
           String[] result = new String[emptyNames.size()];
           return emptyNames.toArray(result);
       }
   
       //封装同名称属性复制，但是空属性不复制过去
       public static void copyPropertiesIgnoreNull(Object src, Object target) {
           BeanUtils.copyProperties(src, target, getNullPropertyNames(src));
       }
   }
   ```

2. 共同常数静态类资源

   在 common 目录下创建 `Constants` 常数类

   ```java
   package com.n303b.gsb_data.common;
   
   public class Constants {
   }
   ```

## 4. 测试

测试项包括

> 1. 测试异常、日志、返回类、uid
> 2. 测试 mybatis、通用 mapper

当然，在具体测试以上各项之前，先启动一下项目，看能否正常启动，没有意外的话是可以正常启动的，启动不了则具体问题具体分析。

*tips：以下步骤仅仅是代码的简单粘贴，并没有考虑前后顺序，所以在没有全部创建好之前，报红是免不了的。*

1. controller 下创建 `DogController`

   ```java
   package com.n303b.gsb_data.controller;
   
   import com.github.wujun234.uid.impl.CachedUidGenerator;
   import com.n303b.gsb_data.exception.CMSException;
   import com.n303b.gsb_data.service.DogService;
   import com.n303b.gsb_data.utils.R;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   import javax.annotation.Resource;
   
   // 众所周知，狗儿是用来测试的，到时关于 Dog 的删掉就好
   @RestController
   @RequestMapping("dog")
   public class DogController {
       public static Logger logger = LoggerFactory.getLogger(DogController.class);
   
       @Resource
       private CachedUidGenerator cachedUidGenerator;
   
       @Resource
       private DogService dogService;
   
       // 测试异常、日志、返回类、uid
       @RequestMapping("test1")
       public R test1() throws Exception {
           int t = 3;
           if (t == 1) {
               throw new CMSException(111, "自定义异常");
           } else if (t == 2) {
               throw new Exception("祖师爷异常");
           }
           logger.info("测试日志: 正常");
           logger.info("测试uid: " + cachedUidGenerator.getUID());
           return R.ok();
       }
   
       // 测试 mybatis、通用 mapper
       @RequestMapping("testMybatis")
       public R testMybatis() {
           // 通用 mapper 插入一条数据
           String id = String.valueOf(cachedUidGenerator.getUID());
           dogService.insertDog(id, "n303b_胥帆（渣男）", 38);
   
           // xml 查询刚插入的数据
           return R.ok().data("Dog", dogService.getDog(id));
       }
   }
   ```

2. service 下创建 `DogService`

   ```java
   package com.n303b.gsb_data.service;
   
   import com.n303b.gsb_data.dao.mapper.DogDao;
   import com.n303b.gsb_data.entity.Dog;
   import org.springframework.stereotype.Service;
   
   import javax.annotation.Resource;
   
   @Service
   public class DogService {
       @Resource
       private DogDao dogDao;
   
       public Dog getDog(String id) {
           return dogDao.getDogById(id);
       }
   
       public void insertDog(String id, String name, Integer age) {
           Dog dog = Dog.builder().id(id).name(name).age(age).build();
           dogDao.insert(dog);
       }
   }
   ```

3. dao.mapper 下创建 `DogDao`

   ```java
   package com.n303b.gsb_data.dao.mapper;
   
   import com.n303b.gsb_data.dao.base.BaseDao;
   import com.n303b.gsb_data.entity.Dog;
   import org.apache.ibatis.annotations.Param;
   
   public interface DogDao extends BaseDao<Dog> {
       public Dog getDogById(@Param("id") String id);
   }
   ```

4. resources/mapper 下创建 `DogDao.xml` 

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <mapper namespace="com.n303b.gsb_data.dao.mapper.DogDao">
       <select id="getDogById" resultType="com.n303b.gsb_data.entity.Dog">
           SELECT *
           FROM dog
           WHERE id = #{id}
       </select>
   </mapper>
   ```

5. entity 下创建 `Dog` ，当然你数据库得有条狗

   ```java
   package com.n303b.gsb_data.entity;
   
   import lombok.Builder;
   import lombok.Data;
   
   import javax.persistence.Column;
   import javax.persistence.Id;
   import javax.persistence.Table;
   
   @Builder
   @Data
   @Table(name = "dog")
   public class Dog {
       @Id
       @Column(name = "id")
       private String id;
       @Column(name = "name")
       private String name;
       @Column(name = "age")
       private Integer age;
   }
   ```

6. 通过 postman 调 controller 的两个接口，或者之间点击 controller 中接口序号旁边像地球一样的东西来调接口也可以。顺利的话，应该是符合预期的，这个预期是什么，得去看上面的具体逻辑，这里不详述。

## 5. 关联到Github

若测试顺利通过，那么恭喜你！一个 **该有的都有了** 的 spring boot web后端项目创建完成。但工作尚未结束，工程嘛！得多人合作，远程仓库自然少不得，故需要**将本地 git 仓库关联到远程仓库**（ GitHub、Gitee、Gitlab 都行，反正得上远程）。

- 用 idea关联到 Github 非常简单

  > VCS -> share project to Github -> 填写信息确定就好
  >
  > 也可以直接上 GitHub 创建一个空的仓库，然后按照提示来关联，这个适用于其他两类 git 网站。

- 后续操作

  有组织团队的话，transfer权限。transfer 之后，远程仓库地址会发生变化，故本地仓库的远程关联仓库地址也得相应更改。

## 6. 写在后面

### 6.1 我还欠你一个完整的 yaml 文件

```yaml
# server
server:
  port: 8088
# datasource
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://47.115.170.16:3306/alertdb?serverTimezone=UTC
    username: yjxt
    password: yjxt@2021
# mybatis
mybatis:
  type-aliases-package: com.n303b.gsb_data.entity # entity
  mapper-locations: mapper/*.xml
  configuration:
    map-underscore-to-camel-case: true
# mapper
mapper:
  mappers: com.n303b.gsb_data.dao.base.BaseDao # base mapper
  identity: MYSQL
# log
logging:
  config: classpath:logback-spring.xml
# minio
minio:
  url: http://127.0.0.1:9000
  accessKey: minioadmin
  secretKey: minioadmin
```

### 6.2 给我自己看的

1. Spring Initializr
2. pom
3. 目录结构
4. yaml
5. 日志、R、异常
6. uid、minio、通用 mapper
7. git