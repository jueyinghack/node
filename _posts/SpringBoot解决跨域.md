---
title: SpringBoot解决跨域问题
date: 2023-05-09 15:41:15
tags: ["java","SpringBoot"]
categories: java
author: sec
---

```java
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
    .cors()
    .configurationSource(this.corsConfigurationSource())
    .and()
    .exceptionHandling()
    .authenticationEntryPoint(this::onAuthenticationFailure)
    .and()
    .build();
}

public CorsConfigurationSource corsConfigurationSource(){
    CorsConfiguration cors = new CorsConfiguration();
    cors.addAllowedOriginPattern("*");
    cors.setAllowCredentials(true);
    cors.addAllowedHeader("*");
    cors.addAllowedMethod("*");
    cors.addExposedHeader("*");
    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/**",cors);
    return source;
}
```