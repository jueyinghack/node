---
title: SpringBoot整合
date: 2023-05-08 16:51:15
tags: ["java","SpringBoot"]
categories: java
author: sec
---
# SpringBoot整合Mybatis-plus

1. 导入依赖

   ```asp
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.4.2</version>
       <relativePath/>
   </parent>
   <dependencies>
       # mysql
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.48</version>
       </dependency>
   	# springboot
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter</artifactId>
       </dependency>
       # springboot-test
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
       # mybatis-plus
       <dependency>
           <groupId>com.baomidou</groupId>
           <artifactId>mybatis-plus-boot-starter</artifactId>
           <version>3.4.3</version>
       </dependency>
       # 测试类
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
           <scope>test</scope>
       </dependency>
    	# druid 连接池   
       <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>druid</artifactId>
           <version>1.2.9</version>
       </dependency>
   </dependencies>
   ```

2. 创建实体类

   ```java
   
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   @ToString
   @TableName("db_account")
   public class Account {
       int id;
       String username;
       String password;
       String email;
   }
   
   ```

3. 配置配置文件

    ```yaml
    # 配置文件
    spring:
      datasource:
        driver-class-name: com.mysql.jdbc.Driver  #定义配置驱动类
        username: root #mysql登录用户名
        password: 123 #mysql登录密码
        url: jdbc:mysql://localhost:12345/shopDB?characterEncoding=utf8&allowMultiQueries=true 
        type: com.alibaba.druid.pool.DruidDataSource #配置连接池
        druid:
          one:
            max-active: 100 #最大连接数
            min-idle: 20 #最小连接数
            max-wait: 2000 #超时时间（ms）
    
    mybatis-plus:
      configuration:
        log-impl: org.apache.ibatis.logging.stdout.StdOutImpl #配置日志
      type-aliases-package: com.project.bean #实体类所在包，允许用实体类类名作为别名
      mapper-locations: classpath:*/*Mapper.xml #链接 mapper文件
    ```

4. 创建业务接口

    ```java
    public interface AccountService {
        public void add(Account account);
        public void del(Integer id);
        public void update(Integer id);
        public List<Account> findAll();
        public Account findById(Integer id);
    }
    ```

5. 创建Mapper接口

    ```java
    @Mapper
    public interface IProductMapper extends BaseMapper<ProductBean> {
    }
    ```

6. 业务实现类

    ```java
    @Service
    @Transactional//该类所有方法支持事务
    public class AccountServiceImpl implements AccountService {
        @Autowired
        private AccountMapper mapper;
        @Override
        public void add(Account account) {
            mapper.insert(account);//添加实体数据
        }
    
        @Override
        public void del(Integer id) {
            mapper.deleteById(id);//根据id删除实体数据
        }
    
        @Override
        public void update(Integer id,String username,String password) {
            Account account = new Account();
            account.setId(id);
            account.setUsername(username);
            account.setPassword(password);
            mapper.updateById(account);
        }
    
        @Override
        public List<Account> findAll() {
            return mapper.selectList(null);//查询所有
        }
    
        @Override
        public Account findById(Integer id) {
            return mapper.selectById(id);//按id查询实体对象
        }
    
    
    ```

7. 启动类

    ```java
    // 启动类
    @SpringBootApplication(scanBasePackages = {"com.sec"})
    @MapperScan("com.sec.mapper")
    public class StudyProjectBackendApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(StudyProjectBackendApplication.class, args);
        }
    
    }
    ```

# SPringBoot整合SpringSecurity

1. 导入依赖

    ```asp
    # SpringBoot-Security
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    ```

2. 配置Config

    ```java
    /**
     * security的配置
     */
    import com.alibaba.fastjson.JSONObject;
    import com.sec.entity.RestBean;
    import com.sec.service.AuthorizeService;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.security.authentication.AuthenticationManager;
    import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.core.Authentication;
    import org.springframework.security.core.AuthenticationException;
    import org.springframework.security.web.SecurityFilterChain;
    
    import javax.annotation.Resource;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;
    
    @Configuration
    @EnableWebSecurity
    public class SecurityConfiguration {
        @Resource
        private AuthorizeService authorizeService;
        @Bean
        public SecurityFilterChain filterChain(HttpSecurity http) throws Exception{
            return http
                    .authorizeHttpRequests()
                    .anyRequest().authenticated()
                    .and()
                    .formLogin()
                    .loginProcessingUrl("/api/auth/login")
                    .successHandler(this::onAuthenticationSuccess)
                    .failureHandler(this::onAuthenticationFailure)
                    .and()
                    .logout()
                    .logoutUrl("/api/auth/logout")
                    .and()
                    .csrf()
                    .disable()
                    .exceptionHandling()
                    .authenticationEntryPoint(this::onAuthenticationFailure)
                    .and()
                    .build();
        }
        @Bean
        public AuthenticationManager authenticationManager(HttpSecurity security) throws Exception{
            return security.getSharedObject(AuthenticationManagerBuilder.class)
                    .userDetailsService(authorizeService)
                    .and()
                    .build();
        }
    
        private void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException e) throws IOException {
            response.setCharacterEncoding("utf-8");
            response.getWriter().write(JSONObject.toJSONString((RestBean.failure(401,e.getMessage()))));
        }
    
        private void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException {
            response.setCharacterEncoding("utf-8");
            response.getWriter().write(JSONObject.toJSONString((RestBean.success())));
        }
    }
    ```

3. 创建实体类

    ```java
    import com.baomidou.mybatisplus.annotation.TableName;
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;
    import lombok.ToString;
    
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    @ToString
    @TableName("db_account")
    public class Account {
        int id;
        String username;
        String password;
        String email;
    }
    
    ```

4. 创建Dao

    ```java
    import com.baomidou.mybatisplus.core.mapper.BaseMapper;
    import com.sec.entity.Account;
    import org.apache.ibatis.annotations.Param;
    import org.apache.ibatis.annotations.Select;
    
    
    
    public interface UserMapper extends BaseMapper<Account> {
        Account findAccountByUsernameOrEmail(@Param("text")String text);
    }
    ```

5. 创建Mapper

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
    <mapper namespace="com.sec.mapper.UserMapper">
        <select id="findAccountByUsernameOrEmail"  resultType="Account">
            select * from db_account where username=#{text} or email=#{text}
        </select>
    
    </mapper>
    ```

6. 创建AuthService

    ```java
    import com.sec.entity.Account;
    import com.sec.mapper.UserMapper;
    import org.springframework.security.core.userdetails.User;
    import org.springframework.security.core.userdetails.UserDetails;
    import org.springframework.security.core.userdetails.UserDetailsService;
    import org.springframework.security.core.userdetails.UsernameNotFoundException;
    import org.springframework.stereotype.Service;
    
    import javax.annotation.Resource;
    @Service
    public class AuthorizeService implements UserDetailsService {
        @Resource
        private UserMapper userMapper;
    
        @Override
        public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
            if(username == null){
                throw new UsernameNotFoundException("用户名不能为空");
            }
            Account account = userMapper.findAccountByUsernameOrEmail(username);
            if(account == null){
                throw new UsernameNotFoundException("用户名或密码错误");
            }
            return User.withUsername(account.getUsername())
                    .password(account.getPassword())
                    .roles("user")
                    .build();
        }
    }
    ```

7. 另附entity

    ```java
    package com.sec.entity;
    
    import lombok.Data;
    
    @Data
    public class RestBean {
        private int code;
        private String message;
        private Object data;
    
        public RestBean(int code,String message,Object data){
            this.code = code;
            this.message = message;
            this.data = data;
        }
    
        public static RestBean success(){
            return new RestBean(200,"success",null);
        }
    
        public static RestBean success(Object data){
            return new RestBean(200,"success",data);
        }
    
        public static RestBean failure(int code){
            return new RestBean(code,"failure",null);
        }
    
        public static RestBean failure(int code,String msg){
            return new RestBean(code,msg,null);
        }
    
    }
    ```

    
