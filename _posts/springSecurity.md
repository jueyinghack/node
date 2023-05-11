---
title: There is no PasswordEncoder mapped for the id “null“ 问题解决
tags: ["java","springsecurity"]
categories: java
date: 2023-5-8 11:52:02
author: sec
---
## There is no PasswordEncoder mapped for the id “null“ 问题解决

> 当我进行springsecurity的测试的时候，我将密码进行了BCryptPasswordEncoder加密，然后将加密的密码直接存入了数据库。当验证密码的时候发现报错

### 原因

- springsecurity从4.2升级到5.0之后加密的方式发生了改变
- 4.2的时候只是使用了BCryptPasswordEncoder进行加密
- 5.0的时候不光使用BCryptPasswordEncoder加密而且需要指定一个encodingId，如果不指定就会报错

```java
package com.xxx.security.test;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.factory.PasswordEncoderFactories;
 
public class PasswordEncoder {
    public static void main(String[] args) {
        org.springframework.security.crypto.password.PasswordEncoder encoder = PasswordEncoderFactories.createDelegatingPasswordEncoder();
        String password = "123456";
        String password2 = encoder.encode(password);
        System.out.println(password2);
        System.out.println(encoder.matches(password, password2));
        BCryptPasswordEncoder encoder2 = new BCryptPasswordEncoder();
        String password3 = encoder2.encode(password);
        System.out.println(password3);
        System.out.println(encoder2.matches(password, password3));
    }  
}
// 可以发现两次的加密结果不同
/**
和以往加密方式不同的是，springsecurity5.0以上版本对bcrypt加密方式进行了随机数处理，所以每次产生的结果都不一样。不管是哪种方式，我们如果使用默认的加密方式，就需要在xml中配置密码为如下的样子。记得前面有{bcrypt}。
{bcrypt}$2a$10$rY/0dflGbwW6L1yt4RVA4OH8aocD7tvMHoChyKY/XtS4DXKr.JbTC
报错的意思是说我们没有指定一个encodingId，最终在encoders map中没有匹配到encodingId。
*/
```

