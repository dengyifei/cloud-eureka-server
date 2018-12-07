server:
  port: 8081
  context-path: /eureka-server
#  eureka server 启动时会创建一个定时任务，每隔一段时间(默认60s)将当前清单中超时(默认90s)没有续约的服务剔除出去
#  自我保护 ：
#  在信息面板中可能会出现这样的红色警告 EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.
#  该警告就是触发了eureka的自我保护机制，服务注册到eureka server后会维护一个心跳连接.eureka server在运行期间，会统计心跳失败的比例在15分钟内是否低于85%.
#  如果低于,会将当前的实例注册信息保护起来,让这些实例不会过期.可以配置 eureka.server.enable-self-preservation=false 来关闭保护机制
#  关闭保护机制，信息面板中会显示 THE SELF PRESERVATION MODE IS TURNED OFF.THIS MAY NOT PROTECT INSTANCE EXPIRY IN CASE OF NETWORK/OTHER PROBLEMS.
eureka:
  server:
    enable-self-preservation: false
  instance:
#   hostname不配置的话，会根据操作系统的主机名来获取
    hostname: localhost
#    注册服务之后，服务提供者会维护一个心跳用来持续告诉eureka server服务正常，以防止eureka server的剔除任务将该服务实例从服务列表中排除出去，该操作称为服务续约
#    lease-renewal-interval-in-seconds: 30     定义服务续约任务的调用间隔时间(默认30s,官方不推荐修改)
#    lease-expiration-duration-in-seconds: 90  定义服务失效的时间(默认90s,官方不推荐修改)
  client:
#  在默认设置下，该服务注册中心也会将自己作为客户端来尝试注册它自己，所以我们需要禁用它的客户端注册行为
#  配置register-with-eureka: false 和 fetch-registry: false 来表明自己是一个eureka server
    register-with-eureka: false
    fetch-registry: false
#  为了性能考虑，eureka server 会维护一份只读的服务清单返回给客户端，同时该缓存清单会每隔30s更新一次(可通过registry-fetch-interval-seconds进行修改)
#    registry-fetch-interval-seconds: 30


server:
  port: 8082
  context-path: /eureka-client
eureka:
  client:
    service-url:
#     配置服务注册中心集群时，此处可以配置多个地址(通过逗号隔开)
      defaultZone: http://localhost:8081/eureka-server/eureka/
# spring.application.name,这个很重要，在以后的服务与服务之间相互调用一般都是根据这个name
spring:
  application:
    name: eureka-client