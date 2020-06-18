### 一、alibaba nacos 作为注册中心

步骤：

#### 1、配置nacos

1. pom文件中引入jar包;版本一定要对应 参考:https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md

   ```xml
    		<!-- cloud alibaba 服务的注册/发现 -->
           <dependency>
               <groupId>com.alibaba.cloud</groupId>
               <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
           </dependency>
   ```

2.  application.properties 中添加注册中心配置

   ```xml
   # nacos 地址
   spring.cloud.nacos.discovery.server-addr=127.0.0.1:8848
   # 名称一定要有 否则 注册不到nacos上
   spring.application.name=member 
   ```

3.  添加 @EnableDiscoveryClient 向注册中心注册

   ```java
   @EnableDiscoveryClient
   public class GulimallMemberApplication {
       public static void main(String[] args) {
           SpringApplication.run(GulimallMemberApplication.class, args);
       }
   }
   ```

#### 2、 配置open-feign 启用远程调用

1. 1. 引入pom文件 open-feign

   ```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-openfeign</artifactId>
   </dependency>
   ```

2.  编写(声明)一个接口，告诉SpringCloud这个接口需要调用远程服务 使用 @FeignClient("gulimall-coupon") 其中value为要调用的服务名

   ``` java
   @SpringBootApplication
   @EnableDiscoveryClient
   @EnableFeignClients(basePackages = "com.atguigu.gulimall.member.feign")
   public class GulimallMemberApplication {
       public static void main(String[] args) {
           SpringApplication.run(GulimallMemberApplication.class, args);
       }
   }
   ```

3.  开启远程调用功能 @EnableFeignClients(basePackages = "com.atguigu.gulimall.member.feign")

```java
// @FeignClient 中的vaule值表示的被调用方在nacos中的注册名
@FeignClient("gulimall-coupon")
@Component
public interface CouponFeignService {
    // 被调用方的全地址
    @RequestMapping("/coupon/coupon/member/list")
    public R membercoupons();
}
```