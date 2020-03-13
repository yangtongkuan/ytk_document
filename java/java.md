## java学习笔记

### 一。springboot使用热部署

1.  添加热部署jar

   ```xml
   <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-devtools</artifactId>
      <scope>runtime</scope>
       <optional>true</optional>
   </dependency>
   ```

2. 修改pom文件

   ```xml
   <!--下一段配置黏贴到父工程当中的pom里-->
   <build>
     <plugins>
       <plugin>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-maven-plugin</artifactId>
         <configuration>
           <fork>true</fork>
           <addResources>true</addResources>
         </configuration>
       </plugin>
     </plugins>
   </build>
   ```

3. 设置自动build

   > idea 项目设置

   ![](.\images\java_idea_setting_01.png)

4. idea设置

   > 使用 ctrl+shift+alt+/ 快捷键打开设置

   ![](.\images\java_idea_setting_02.png)

   ![](.\images\java_idea_setting_03.png)

5. 重启idea(可有可无 具体看自己的情况)