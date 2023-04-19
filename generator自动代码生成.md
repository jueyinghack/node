步骤

1. 导入依赖

   ```apl
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>8.0.15</version>
               <!--<scope>runtime</scope>-->
           </dependency>
           <!--mybatis-plus 从数据库生成后端代码,直到controller-->
           <dependency>
               <groupId>com.baomidou</groupId>
               <artifactId>mybatis-plus-boot-starter</artifactId>
               <version>3.5.1</version>
           </dependency>
           <!--mybatis-plus-generator 代码生成器-->
           <dependency>
               <groupId>com.baomidou</groupId>
               <artifactId>mybatis-plus-generator</artifactId>
               <version>3.5.2</version>
           </dependency>
           <!--代码生成器所需的模板-->
           <dependency>
               <groupId>org.freemarker</groupId>
               <artifactId>freemarker</artifactId>
               <version>2.3.31</version>
           </dependency>
            <!--lombok-->
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
           </dependency>
           <dependency>
               <groupId>io.swagger</groupId>
               <artifactId>swagger-annotations</artifactId>
               <version>1.5.22</version>
           </dependency>
   ```

2. 创建Generator类

   ```java
   package com.sec.demo.gengrator;
   
   import com.baomidou.mybatisplus.generator.FastAutoGenerator;
   import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
   import com.baomidou.mybatisplus.generator.config.GlobalConfig;
   import com.baomidou.mybatisplus.generator.config.PackageConfig;
   import com.baomidou.mybatisplus.generator.config.TemplateConfig;
   import com.baomidou.mybatisplus.generator.config.rules.DateType;
   import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;
   
   public class Generator {
       //  数据库配置
       private static final DataSourceConfig.Builder DATA_SOURCE_CONFIG = new DataSourceConfig
               .Builder(
               "jdbc:mysql://localhost:3306/studymybatisplus?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai"
               , "root"
               , "qwe7744066"
       );
   
       //  全局配置
       private static GlobalConfig.Builder globalConfig() {
           return new GlobalConfig.Builder()
                   .outputDir("/src/main/java")
                   .author("wang")
                   .enableSwagger()
                   .enableKotlin()
                   .dateType(DateType.TIME_PACK)
                   .commentDate("yyyy-MM-dd");
       }
   
       //    包配置
       private PackageConfig.Builder packageConfig() {
           return new PackageConfig.Builder()
                   .parent("com.sec.demo")
                   .entity("entity")
                   .service("service")
                   .serviceImpl("service.impl")
                   .mapper("mapper")
                   .controller("controller");
       }
   
       //    模板配置
       private TemplateConfig.Builder templateConfig() {
           return new TemplateConfig.Builder();
       }
   
   
       public static void main(String[] args) {
           String projectPath = System.getProperty("user.dir");
           FastAutoGenerator.create(DATA_SOURCE_CONFIG)
                   .globalConfig((builder) -> {
                       builder.author("wang")
                               .enableSwagger()
                               .outputDir(projectPath + "/src/main/java");
                   })
                   .packageConfig((builder) -> {
                       builder.parent("com.sec.demo");
                   })
                   .strategyConfig((scanner, builder) -> {
                       builder.addInclude(scanner.apply("请输入表名,多个表用,隔开"))
                               .addTablePrefix("")
                               .entityBuilder()
                               .enableLombok()
                               .mapperBuilder()
                               .enableBaseColumnList()
                               .enableBaseResultMap()
                               .controllerBuilder()
                               .enableRestStyle();
                   })
                   .templateEngine(new FreemarkerTemplateEngine())
                   .execute();
       }
   }
   
   ```
   
3. 运行Generator类