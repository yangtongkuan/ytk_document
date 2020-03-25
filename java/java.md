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

### 三、linux下生成分享图片包含中文乱码问题

#### 1. **产生原因**

> linux下没有微软字体，当设置字体为微软字体时，会出现字体样式找不到

```java
// For Example
Font font2 = new Font("微软雅黑", Font.PLAIN, bean.getFontSize());// 添加字体的属性设置
```

#### 2. 解决方式

>  参考博客 ：https://blog.csdn.net/weixin_39917216/article/details/81698284

++++

### 四、idea下.gitignore文件不生效的解决方式

```sh
# 1.清除本地缓存
git rm -r --cached .
# 2. 重新加入 
git add .
# 3. 提交到本地并提交到仓库
git commit -m '.gitignore modify'
#
git push origin master
```

