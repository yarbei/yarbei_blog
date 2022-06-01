## 搭建maven项目

---

### abbrlink: 2

> controller一般都是对接前端的 每个方法上都有一个路径  比如说删除接口  方法上就会来个@RequestMapping("/delete")，把这个接口暴露出来；service是服务类；mapper是最后一层，和数据库连接，用于获取数据；entity包 就是当我们写新增或者修改sql的时候 一般用这个实体类，或者用于接收前端的参数；

1.安装maven

安装教程：[https://blog.csdn.net/u012913466/article/details/104150302](https://blog.csdn.net/u012913466/article/details/104150302)

2.创建maven项目，勾选create from archetype，选择maven_archetype-quickstart

![1601612705832-c55defa6-40cb-4ad0-aa7b-11156ac687b3.png](assets/1601612705832-c55defa6-40cb-4ad0-aa7b-11156ac687b3.png)

3.选择覆盖maven的settings.xml，选择maven下载的包的位置

![1601612757155-3d9cb5a2-7bdf-4960-ac98-bc2c9d982851.png](assets/1601612757155-3d9cb5a2-7bdf-4960-ac98-bc2c9d982851.png)

4.创建好maven项目，修改pom.xml，替换依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.yabei.demo</groupId>
  <artifactId>JavaDemo</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>JavaDemo</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.6.RELEASE</version>
  </parent>
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-core</artifactId>
      <version>1.3.5</version>
    </dependency>

    <!-- 添加fastjson 依赖包. -->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>fastjson</artifactId>
      <version>1.2.15</version>
    </dependency>
    <!-- mysql 数据库驱动. -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
    </dependency>


    <!-- servlet 依赖. -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <scope>provided</scope>
    </dependency>

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
    </dependency>

    <!-- tomcat 的支持.-->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.tomcat.embed</groupId>
      <artifactId>tomcat-embed-jasper</artifactId>
      <scope>provided</scope>
    </dependency>


    <!-- spring boot devtools 依赖包. -->
<!--    <dependency>-->
<!--      <groupId>org.springframework.boot</groupId>-->
<!--      <artifactId>spring-boot-devtools</artifactId>-->
<!--      <optional>true</optional>-->
<!--      <scope>true</scope>-->
<!--    </dependency>-->

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-freemarker</artifactId>
    </dependency>

    <dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>4.1.6</version>
    </dependency>


    <!-- 阿里巴巴连接池 -->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.6</version>
    </dependency>


    <!-- https://mvnrepository.com/artifact/org.apache.velocity/velocity-engine-core -->
    <dependency>
      <groupId>org.apache.velocity</groupId>
      <artifactId>velocity-engine-core</artifactId>
      <version>2.0</version>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <optional>true</optional>
    </dependency>
    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>3.0.6</version>
    </dependency>
  </dependencies>

  <build>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```

5.修改启动类

```java
package com.yabei.demo;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;


//根据添加的jar依赖自动配置Spring应用
@EnableAutoConfiguration
//表示将该类自动扫描并注册为Bean.可以自动收集所有的Spring组件,包括@Congfiguration
@ComponentScan
@Configuration

public class Home
{
    public static void main(String args[]){
        SpringApplication.run(Home.class,args);
    }
}
```

6.main下面创建resources文件夹，resources下创建application.yml文件，配置端口号：

```java
#SpringBoot 启动端口和项目路径server:server:
server:
  port: 8080
  servlet:
    context-path: /
```

7.启动MySQL，打开Navicat，创建连接，新建数据库，数据表，字符集utf8,排序规则utf8_bin

8.application.yml新增：

```java
spring:
  #数据源
  datasource:
    #数据库
    url: jdbc:mysql://localhost:3306/java_demo_db?characterEncoding=UtF-8
    #数据库用户名
    username: root
    #数据库密码
    password:
    driver-class-name: com.mysql.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
        
 
#mybatis-plus配置
mybatis-plus:
  #扫描的mapper位置,mapper-locations  指的是项目启动时要扫描的mapper位置，  mapper里面是写sql的
  mapper-locations: classpath:mapper/*.xml
  #别名配置
  type-aliases-package: com.yabei.entity
```

9.创建entity包，controller包，service包，mapper包，分别创建Student类，StudentController类，StudentService接口，StudentMapper接口，在resources下创建mapper包

10.StudentController实体类加@Controller注解，StudentMapper加@RequestMapping("/student")注解

11.service包下创建impl包并且创建实体类StudentServiceImpl

```java
package com.yabei.service.impl;

import com.yabei.service.StudentService;
import org.springframework.stereotype.Service;

@Service
public class StudentServiceImpls implements StudentService {

}
```

12.StudentMapper接口增加@Mapper注解

13.resources 的mapper包下面新建StudentMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace 一般都是用来放mapper下面接口的路径 -->
<mapper namespace="com.yabei.mapper.StudentMapper">


</mapper>
```

