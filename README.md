# springcloud-study
springcloud的学习记录和demo工程

## 阶段一：创建服务和客户端（Eureka）
      大致了解springcloud，知道它是一个构建微服务架构的工具后，就可以开始创建这些来练手了。
      
### 创建Eureka server

通过IDEA 创建新工程，选择spring Assinstant(有版本名Initializr)，选择cloud discovery中的eureka server，直接下一步到创建完成。
在启动类添加@EnableEurekaServer，加上配置文件，就可以启动了。

      server:
      port: 8765

    eureka:
      instance:
        hostname: localhost
      client:
        registerWithEureka: false
        fetchRegistry: false
        serviceUrl:
          defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/

成功启动后可以通过localhost:8765访问eureka的服务端。

对应代码：springcloud_demo

### 创建Eureka client
客户端创建一样，只是选择的时候选择cloud discovery中的eureka client.
启动文件加上@EnableEurekaClient表自己是一个客户端，可以被发现。配置文件中连接服务端。

            eureka:
              client:
                serviceUrl:
                  defaultZone: http://localhost:8765/eureka/
            server:
              port: 8765
            spring:
              application:
                name: demo

两边都启动后，可以看到Instances currently registered with Eureka有了刚才的demo应用。





