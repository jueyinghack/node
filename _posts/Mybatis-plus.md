---
title: Mybatis-plus
date: 2023-05-10 17:15:15
tags: ["java","SpringBoot","Mybatis-plus"]
categories: java
author: sec
---
# Mybatis-plus

## 简介

> mybatis-plus是mybatis的升级版，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

## 特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

## 使用

1. 创建数据库与表

    ```sql
    # 创建数据库
    create database mybatis_plus_study;
    use mybatis_plus_study;
    # 创建表
    DROP TABLE IF EXISTS user;
    CREATE TABLE user
    (
        id BIGINT(20) NOT NULL COMMENT '主键ID',
        name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
        age INT(11) NULL DEFAULT NULL COMMENT '年龄',
        email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
        PRIMARY KEY (id)
    );
    
    # 插入数据
    DELETE FROM user;
    INSERT INTO user (id, name, age, email) VALUES
    (1, 'Jone', 18, 'test1@baomidou.com'),
    (2, 'Jack', 20, 'test2@baomidou.com'),
    (3, 'Tom', 28, 'test3@baomidou.com'),
    (4, 'Sandy', 21, 'test4@baomidou.com'),
    (5, 'Billie', 24, 'test5@baomidou.com');
    
    
    ```

    

3. 创建一个SpringBoot项目，导入mybatis-plus依赖

    ```asp
    <!-- pom文件如下 -->
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.2.1.RELEASE</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>
        <groupId>com.sec</groupId>
        <artifactId>mybatis-plus-study</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <name>mybatis-plus-study</name>
        <description>mybatis-plus-study</description>
        <properties>
            <java.version>1.8</java.version>
        </properties>
        <dependencies>
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
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
    
            <!--mybatis-plus-->
            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-boot-starter</artifactId>
                <version>3.3.1</version>
            </dependency>
    
            <!--mysql-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
            </dependency>
    
            <!--lombok用来简化实体类-->
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
            </dependency>
        </dependencies>
    
        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <configuration>
                        <excludes>
                            <exclude>
                                <groupId>org.projectlombok</groupId>
                                <artifactId>lombok</artifactId>
                            </exclude>
                        </excludes>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    
    </project>
    
    
    ```
    
3. 配置配置文件

    ```yml
    # DataSource Config
    spring:
      datasource:
        driver-class-name: com.mysql.cj.jdbc.Driver
        username: root
        password: root
        url: jdbc:mysql://localhost:3306/mybatis_plus_study?serverTimezone=GMT%2B8
    
    ```
    
    
    
4. 在启动类上增加注解

    ```java
    package com.sec;
    
    import org.mybatis.spring.annotation.MapperScan;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    @SpringBootApplication
    @MapperScan("com.sec.mapper")
    public class MybatisPlusStudyApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(MybatisPlusStudyApplication.class, args);
        }
    
    }
    ```

5. 创建实体类

    ```java
    package com.sec.entity;
    
    import lombok.Data;
    
    @Data
    public class User {
        private Long id;
        private String name;
        private Integer age;
        private String email;
    }
    
    ```

6. 创建UserMapper

    ```java
    package com.sec.mapper;
    
    import com.baomidou.mybatisplus.core.mapper.BaseMapper;
    import com.sec.entity.User;
    
    public interface UserMapper extends BaseMapper<User> {
    }
    
    ```

7. 测试

    ```java
    package com.sec;
    
    import com.sec.entity.User;
    import com.sec.mapper.UserMapper;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.context.SpringBootTest;
    
    import javax.annotation.Resource;
    import java.util.Arrays;
    import java.util.List;
    
    @SpringBootTest
    class MybatisPlusStudyApplicationTests {
    
        @Resource
        private UserMapper userMapper;
        @Test
        void contextLoads() {
            // 根据id查询记录
            User userOne = userMapper.selectById(1);
            System.out.println(userOne);
    
            // 查询所有记录
            List<User> users = userMapper.selectList(null);
            for(User user: users){
                System.out.println(user);
            }
    
            // 插入
            User insertUser = new User();
            insertUser.setAge(11);
            insertUser.setName("name");
            insertUser.setEmail("973013625@qq.com");
            userMapper.insert(insertUser);
    
            // 根据id更新
            userOne.setName("update_name");
            userMapper.updateById(userOne);
    
            // 根据id删除
            userMapper.deleteById(4);
    
            // 批量删除
            userMapper.deleteBatchIds(Arrays.asList(8,9,10));
    
            // 逻辑删除
            /**
             * - 物理删除：真实删除，将对应数据从数据库中删除，之后查询不到此条被删除数据
             * - 逻辑删除：假删除，将对应数据中代表是否被删除字段状态修改为“被删除状态”，之后在数据库中仍旧能看到此条数据记录
             * 数据库添加deleted字段 ALTER TABLE `user` ADD COLUMN `deleted` boolean
             * 实体类加入deleted字段
             * 在deleted字段上加上@TableLogic注解
             * application.yml加入配置
             * mybatis-plus:global-config:db-config:logic-delete-value=1
             * mybatis-plus.global-config.db-config.logic-not-delete-value=0
             */
            int result = userMapper.deleteById(1L);
    
        }
    
    }
    
    ```



## 分页插件

1. 新建一个config包，在下边新建MpConfig

   ```java
   package com.sec.config;
   
   import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   @Configuration
   public class MpConfig {
       @Bean
       public PaginationInterceptor paginationInterceptor(){
           return new PaginationInterceptor();
       }
   }
   ```

2. 测试

   ```java
   Page<User> page = new Page<>(1,3);
   userMapper.selectPage(page,null);
   page.getRecords().forEach(System.out::println);
   System.out.println(page.getCurrent());
   System.out.println(page.getPages());
   System.out.println(page.getSize());
   System.out.println(page.getTotal());
   System.out.println(page.hasNext());
   System.out.println(page.hasPrevious());
   ```

   

## QueryWrapper

```java
// ge gt le lt （大于，大于等于，小于，小于等于）
// eq ne (等于，不等于)
// like likeLeft likeRight （匹配，左匹配，右匹配）
```



## 封装Service

1. 创建Service

    ```java
    package com.sec.service;
    
    import com.baomidou.mybatisplus.extension.service.IService;
    import com.sec.entity.User;
    
    public interface UserService extends IService<User> {
    }
    ```

2. 创建ServiceImpl

    ```java
    package com.sec.service.impl;
    
    import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
    import com.sec.entity.User;
    import com.sec.mapper.UserMapper;
    import com.sec.service.UserService;
    import org.springframework.stereotype.Service;
    
    @Service
    public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
    }
    ```

    

