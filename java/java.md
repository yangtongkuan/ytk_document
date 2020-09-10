## java学习笔记

### 一、springboot使用热部署

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

+++++

### 五、springboot项目jar 启动脚本

#### 1. 编写启动脚本 如下

```shell
#!/bin/bash
#这里可替换为你自己的执行程序，其他代码无需更改
APP_NAME=xxx-1.0-SNAPSHOT.jar
APP_LOG_NAME=log-xx.out
 
#使用说明，用来提示输入参数
usage() {
    echo "Usage: sh pay.sh [start|stop|restart|status]"
    exit 1
}
 
#检查程序是否在运行
is_exist(){
  pid=`ps -ef|grep $APP_NAME|grep -v grep|awk '{print $2}' `
  #如果不存在返回1，存在返回0     
  if [ -z "${pid}" ]; then
   return 1
  else
    return 0
  fi
}
 
#启动方法
start(){
  is_exist
  if [ $? -eq "0" ]; then
    echo "${APP_NAME} is already running. pid=${pid} ."
  else
    nohup java -jar $APP_NAME > $APP_LOG_NAME 2>&1 &
  fi
}
 
#停止方法
stop(){
  is_exist
  if [ $? -eq "0" ]; then
    kill -9 $pid
  else
    echo "${APP_NAME} is not running"
  fi  
}
 
#输出运行状态
status(){
  is_exist
  if [ $? -eq "0" ]; then
    echo "${APP_NAME} is running. Pid is ${pid}"
  else
    echo "${APP_NAME} is NOT running."
  fi
}
 
#重启
restart(){
  stop
  start
}
 
#根据输入参数，选择执行对应方法，不输入则执行使用说明
case "$1" in
  "start")
    start
    ;;
  "stop")
    stop
    ;;
  "status")
    status
    ;;
  "restart")
    restart
    ;;
  *)
    usage
    ;;
esac

```

#### 2.将脚本和jar文件放在同一文件下 执行如下命令

```shell
# 设置脚本文件可以执行
chmod +x xxx.sh 
# 启动命令
./xxx.sh start
# 查看运行状态
./xxx.sh status
# 停止运行
./xxx.sh stop
# 重启
./xxx.sh restart
# 查看项目启动日志
tail -f log-xxx.out
```

### 六  springboot使用logback日志文件输出配置

***springboot推荐使用 logback-spring.xml作为日志的配置文件***

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
scan：当此属性设置为true时，配置文件如果发生改变，将会被重新加载，默认值为true。
scanPeriod：设置监测配置文件是否有修改的时间间隔，如果没有给出时间单位，默认单位是毫秒当scan为true时，此属性生效。默认的时间间隔为1分钟。
debug：当此属性设置为true时，将打印出logback内部日志信息，实时查看logback运行状态。默认值为false。
-->
<configuration scan="true" scanPeriod="60 seconds" debug="false">
    <!-- 定义日志的根目录 -->
    <springProperty scope="context" name="LOG_HOME" source="logging.path"/>
    <!--<property name="LOG_HOME" value="F:\\logs"/>-->
    <!-- 定义日志文件名称 -->
    <!--<property name="appName" value="logback"></property>-->
    <springProperty scope="context" name="appName" source="spring.application.name"/>

    <!-- 定义输出格式 -->
    <!--<property name="logging.pattern" value="%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n"></property>-->
    <springProperty scope="context" name="logging.pattern" source="logging.pattern.file"/>
    <!-- 定义文件名最大天数 -->
    <!--<property name="log.maxHistory" value="30"></property>-->
    <springProperty scope="context" name="log.maxHistory" source="logging.maxHistory"/>
    <!-- ch.qos.logback.core.ConsoleAppender 表示控制台输出 -->
    <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!--
        日志输出格式：
			%d表示日期时间，
			%thread表示线程名，
			%-5level：级别从左显示5个字符宽度
			%logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
			%msg：日志消息，
			%n是换行符
        -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <springProfile name="dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
            <springProfile name="!dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
        </layout>
    </appender>

    <!-- 滚动记录文件，先将日志记录到指定文件，当符合某个条件时，将日志记录到其他文件 -->
    <appender name="logAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 指定日志文件的名称 -->
        <file>${LOG_HOME}/${appName}.log</file>
        <!--
        当发生滚动时，决定 RollingFileAppender 的行为，涉及文件移动和重命名
        TimeBasedRollingPolicy： 最常用的滚动策略，它根据时间来制定滚动策略，既负责滚动也负责出发滚动。
        -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--
            滚动时产生的文件的存放位置及文件名称 %d{yyyy-MM-dd}：按天进行日志滚动 
            %i：当文件大小超过maxFileSize时，按照i进行文件滚动
            -->
            <fileNamePattern>${LOG_HOME}/${appName}-%d{yyyy-MM-dd}-%i.log</fileNamePattern>
            <!-- 
            可选节点，控制保留的归档文件的最大数量，超出数量就删除旧文件。假设设置每天滚动，
            且maxHistory是30，则只保存最近30天的文件，删除之前的旧文件。注意，删除旧文件是，
            那些为了归档而创建的目录也会被删除。
            -->
            <maxHistory>${log.maxHistory}</maxHistory>
            <!-- 
            当日志文件超过maxFileSize指定的大小是，根据上面提到的%i进行日志文件滚动 注意此处配置SizeBasedTriggeringPolicy是无法实现按文件大小进行滚动的，必须配置timeBasedFileNamingAndTriggeringPolicy
            -->
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
        <!-- 日志输出格式： -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <!--<pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [ %thread ] - [ %-5level ] [ %logger{50} : %line ] - %msg%n</pattern>-->
            <pattern>${logging.pattern}</pattern>
        </layout>
    </appender>
    <!--&lt;!&ndash; 2.2 level为 INFO 日志，时间滚动输出  &ndash;&gt;-->
    <!--<appender name="infoAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">-->
        <!--&lt;!&ndash; 正在记录的日志文档的路径及文档名 &ndash;&gt;-->
        <!--<file>${LOG_HOME}/web_info.log</file>-->
        <!--&lt;!&ndash;日志文档输出格式&ndash;&gt;-->
        <!--<encoder>-->
            <!--&lt;!&ndash;<pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>&ndash;&gt;-->
            <!--<pattern>${logging.pattern}</pattern>-->
            <!--<charset>UTF-8</charset>-->
        <!--</encoder>-->
        <!--&lt;!&ndash; 日志记录器的滚动策略，按日期，按大小记录 &ndash;&gt;-->
        <!--<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">-->
            <!--&lt;!&ndash; 每天日志归档路径以及格式 &ndash;&gt;-->
            <!--<fileNamePattern>${LOG_HOME}/${appName}-web-info-%d{yyyy-MM-dd}.%i.log</fileNamePattern>-->
            <!--<timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">-->
                <!--<maxFileSize>100MB</maxFileSize>-->
            <!--</timeBasedFileNamingAndTriggeringPolicy>-->
            <!--&lt;!&ndash;日志文档保留天数&ndash;&gt;-->
            <!--<maxHistory>${log.maxHistory}</maxHistory>-->
        <!--</rollingPolicy>-->
        <!--&lt;!&ndash; 此日志文档只记录info级别的 &ndash;&gt;-->
        <!--<filter class="ch.qos.logback.classic.filter.LevelFilter">-->
            <!--<level>info</level>-->
            <!--<onMatch>ACCEPT</onMatch>s-->
            <!--<onMismatch>DENY</onMismatch>-->
        <!--</filter>-->
    <!--</appender>-->
    <!-- 2.3 level为 ERROR 日志，时间滚动输出  -->
    <appender name="errorAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 正在记录的日志文档的路径及文档名 -->
        <file>${LOG_HOME}/${appName}_error.log</file>
        <!--日志文档输出格式-->
        <encoder>
            <!--<pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>-->
            <pattern>${logging.pattern}</pattern>
            <charset>UTF-8</charset> <!-- 此处设置字符集 -->
        </encoder>
        <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_HOME}/${appName}-web-error-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <!--日志文档保留天数-->
            <maxHistory>${log.maxHistory}</maxHistory>
        </rollingPolicy>
        <!-- 此日志文档只记录ERROR级别的 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>ERROR</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>


    <!-- 
		logger主要用于存放日志对象，也可以定义日志类型、级别
		name：表示匹配的logger类型前缀，也就是包的前半部分
		level：要记录的日志级别，包括 TRACE < DEBUG < INFO < WARN < ERROR
		additivity：作用在于children-logger是否使用 rootLogger配置的appender进行输出，
		false：表示只用当前logger的appender-ref，true：
		表示当前logger的appender-ref和rootLogger的appender-ref都有效
    -->
    <!-- hibernate logger -->
    <!--<logger name="com.stu" level="debug" />-->
    <!-- Spring framework logger -->
    <!--<logger name="org.springframework" level="debug" additivity="false"></logger>-->


    <!--
    root与logger是父子关系，没有特别定义则默认为root，任何一个类只会和一个logger对应，
    要么是定义的logger，要么是root，判断的关键在于找到这个logger，然后判断这个logger的appender和level。 
    -->
    <!-- 异步输出所有的日志 -->
    <appender name="asyncStdoutAppender" class="ch.qos.logback.classic.AsyncAppender">
        <!-- 不丢失日志.默认的,如果队列的80%已满,则会丢弃TRACT、DEBUG、INFO级别的日志 -->
        <discardingThreshold>0</discardingThreshold>
        <!-- 更改默认的队列的深度,该值会影响性能.默认值为256 -->
        <queueSize>256</queueSize>
        <!-- 添加附加的appender,最多只能添加一个 -->
        <appender-ref ref="stdout"/>
    </appender>
    <!-- 异步记录所有的日志-->
    <appender name="asyncLogAppender" class="ch.qos.logback.classic.AsyncAppender">
        <!-- 不丢失日志.默认的,如果队列的80%已满,则会丢弃TRACT、DEBUG、INFO级别的日志 -->
        <discardingThreshold>0</discardingThreshold>
        <!-- 更改默认的队列的深度,该值会影响性能.默认值为256 -->
        <queueSize>256</queueSize>
        <!-- 添加附加的appender,最多只能添加一个 -->
        <appender-ref ref="logAppender"/>
    </appender>
    <!-- 异步记录所有的日志-->
    <appender name="asyncErrorAppender" class="ch.qos.logback.classic.AsyncAppender">
        <!-- 不丢失日志.默认的,如果队列的80%已满,则会丢弃TRACT、DEBUG、INFO级别的日志 -->
        <discardingThreshold>0</discardingThreshold>
        <!-- 更改默认的队列的深度,该值会影响性能.默认值为256 -->
        <queueSize>256</queueSize>
        <!-- 添加附加的appender,最多只能添加一个 -->
        <appender-ref ref="errorAppender"/>
    </appender>
    <!-- 非异步日志 -->
    <!--<root level="info">-->
    <!--<appender-ref ref="stdout"/>-->
    <!--<appender-ref ref="logAppender"/>-->
    <!--<appender-ref ref="infoAppender"/>-->
    <!--<appender-ref ref="errorAppender"/>-->
    <!--</root>-->
    <!-- 异步日志 -->
    <root level="info">
        <appender-ref ref="asyncStdoutAppender"/>
        <appender-ref ref="asyncLogAppender"/>
        <appender-ref ref="asyncErrorAppender"/>
    </root>
</configuration> 
```

