---
title: spring boot自定义配置文件
date: 2018-09-10 16:04:50
tags: spring
---
配置类
```java
@Data
@Component
@ConfigurationProperties(prefix = "coin.stc")
@PropertySource("classpath:config/coins.properties")
public class StcProperties {
    /**
     * 协议
     */
    private String protocol;
    /**
     * 主机
     */
    private String host;
    /**
     * 端口
     */
    private String port;
    /**
     * token
     */
    private String accesstoken;

}
```
配置文件`config/coins.properties`
```ini
# stc
coin.stc.protocol = http
coin.stc.host = 192.168.102.246
coin.stc.port = 50011
coin.stc.accesstoken = 86349b9c59f85a97f2bbce808d200d60
```
使用下面方式使自定义配置文件使用profile
```java
@PropertySource("classpath:config/coins${spring.profiles.active}.properties")
```
测试配置是否导入
```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class StcJSONRPCClientTest {

    @Autowired
    private StcProperties stcProperties;

    @Test
    public void getClient() {
        System.out.println(stcProperties.getAccesstoken());
        assertEquals("86349b9c59f85a97f2bbce808d200d60",stcProperties.getAccesstoken());
    }
}
```
参考  
[***How to add profile specific properties files when I have custom name to my property file**](https://stackoverflow.com/questions/43179240/how-to-add-profile-specific-properties-files-when-i-have-custom-name-to-my-prope)
[Springboot not loading application.dev.properties file
](https://stackoverflow.com/questions/38042035/springboot-not-loading-application-dev-properties-file)  
[Springboot 之 自定义配置文件及读取配置文件](https://blog.csdn.net/zsl129/article/details/52880798)  
[***How to load property file based on spring profiles**](https://stackoverflow.com/questions/41263105/how-to-load-property-file-based-on-spring-profiles)  
[SpringBoot 读取配置文件及profiles切换配置文件](https://blog.csdn.net/qq_29614459/article/details/71486708)  
[***深入Spring Boot (三)：Properties属性配置文件使用详解**](https://juejin.im/post/5a9be3cb51882555627cbc1f)  
[spring boot 多环境配置读取属性文件](https://blog.csdn.net/u012100371/article/details/78081634)  