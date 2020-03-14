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

++++



### 二、springboot项目使用 **Run Dashboard** 

> 两种方式

* 方式一

1.  找到.idea下面的workspace.xml文件  

   ![](.\images\java_idea_setting_004.png)

2.  在下面的代码中加入一段配置代码  源代码位置 

```xml
<component name="RunDashboard">
  <option name="ruleStates">
      <list>
        <RuleState>
          <option name="name" value="ConfigurationTypeDashboardGroupingRule" />
        </RuleState>
        <RuleState>
          <option name="name" value="StatusDashboardGroupingRule" />
        </RuleState>
      </list>
   </option>
    <option name="contentProportion" value="0.22874807" />
  </component>
```

> 添加如下配置

```xml
<option name="configurationTypes">
      <set>
        <option value="SpringBootApplicationConfigurationType" />
      </set>
</option>
```

3. 大功告成，idea启动后Run Dashboard 在项目中自动启动。 

```xml
<component name="RunDashboard">
 <option name="ruleStates">
     <list>
       <RuleState>
           <option name="name" value="ConfigurationTypeDashboardGroupingRule" />
       </RuleState>
       <RuleState>
          <option name="name" value="StatusDashboardGroupingRule" />
       </RuleState>
     </list>
  </option>
  <option name="contentProportion" value="0.22874807" />
  <option name="configurationTypes">
     <set>
          <option value="SpringBootApplicationConfigurationType" />
     </set>
  </option>
 </component>
```

* 方式二

  > **view ---> Tools Windows---->  Run Dashboard** 

  ![](.\images\java_idea_setting05.png)