# springcloud-study
springcloud的学习记录和demo工程。基于https://github.com/forezp/SpringCloudLearning的实践

## 阶段一：服务的创建(server)和发现（client）：Eureka
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

对应代码：eureka_server

### 创建Eureka client
客户端创建一样，只是选择的时候选择cloud discovery中的eureka discovery.
启动文件加上@EnableEurekaClient表自己是一个客户端，可以被发现。配置文件中连接服务端。

            eureka:
              client:
                serviceUrl:
                  defaultZone: http://localhost:8765/eureka/
            server:
              port: 8083
            spring:
              application:
                name: demo

两边都启动后，可以看到Instances currently registered with Eureka有了刚才的demo应用。
对应代码中的eurekaclient


## 阶段二：服务消费者：rest+ribbon+feign

            微服务架构中，业务都会被拆分成一个独立的服务，服务与服务的通讯是基于http restful的。
            Spring cloud有两种服务调用方式，一种是ribbon+restTemplate，另一种是feign。

### 负载均衡服务 ribbon
创建一个新的客户端ribbon，选择cloud discovery中的eureka discovery，以及web,建立一套controller和service。
启动类中添加@EnableDiscoveryClient，并引入RestTemplate,为了一会调用服务。

            @Bean
            @LoadBalanced
            RestTemplate restTemplate() {
                  return new RestTemplate();
            }

在service实现中，通过如下形式，调用服务：

            restTemplate.getForObject("http://CLIENTHI/hi?name="+name,String.class)
即：
   * 通过之前注入ioc容器的restTemplate来消费service-hi服务的“/hi”接口，
   * 在这里我们直接用的程序名替代了具体的url地址，
   * 在ribbon中它会根据服务名来选择具体的服务实例，根据服务实例在请求的时候会用具体的url替换掉服务名
 

启动多个CLIENTHI，启动ribbon后，访问设定的controller路径，可以验证：ribbon是一个负载均衡客户端，可以很好的控制htt和tcp的一些行为。
对应代码：ribbon_server
