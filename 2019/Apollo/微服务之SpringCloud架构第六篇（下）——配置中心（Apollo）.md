Apollo客户端依赖于AppId，Apollo Meta Server等环境信息来工作，所以请确保下面的配置正确：
**1、 AppId**
AppId是应用的身份信息，是配置中心的一个项目id，一般和应用名称保持一致，是从服务端获取配置的一个重要信息。

有以下3种方式设置，按照优先级从高到底分别为：

1、System Property

   通过System Property传入app.id信息，如：-Dapp.id=YOUR-APP-ID

2、Spring Boot application.properties（本案例使用的方式）

通过Spring Boot的application.properties文件配置，如：app.id=YOUR-APP-ID

3、app.properties

确保classpath:/META-INF/app.properties文件存在，并且其中内容形如：app.id=YOUR-APP-ID

**2、Apollo Meta Server**
Apollo支持应用在不同的环境有不同的配置，所以需要在运行提供给Apollo客户端当前环境信息。默认情况下，meta server和config service是部署在同一个JVM进程，所以meta server的地址就是config service的地址。

支持以下方式配置apollo meta server信息，按照优先级从高到底分别为：

1、通过Java System Property apollo.meta

可以通过Java的System Property apollo.meta来指定

启动脚本中，格式为： -Dapollo.meta=http://config-service-url

运行jar文件，格式为：java -Dapollo.meta=http://config-service-url -jar xxx.jar

程序指定，如System.setProperty("apollo.meta", "http://config-service-url");

2、通过Spring Boot的配置文件（本案例使用的方式）

可以在Spring Boot的application.properties或bootstrap.properties中指定apollo.meta=http://config-service-url

3、通过操作系统的System Environment  APOLLO_META

可以通过操作系统的System Environment  APOLLO_META来指定，key为全大写，且中间是_分隔

4、通过server.properties配置文件

可以在server.properties配置文件中指定apollo.meta=http://config-service-url

对于Mac/Linux，文件位置为/opt/settings/server.properties

对于Windows，文件位置为C:\opt\settings\server.properties

5、通过app.properties配置文件

可以在classpath:/META-INF/app.properties指定apollo.meta=http://config-service-url

6、通过Java system property  ${env}_meta

7、如果当前env是dev，那么用户可以配置-Ddev_meta=http://config-service-url

8、通过apollo-env.properties文件

用户也可以创建一个apollo-env.properties，放在程序的classpath下，或者放在spring boot应用的config目录下
``
dev.meta=http://1.1.1.1:8080
fat.meta=http://apollo.fat.xxx.com
uat.meta=http://apollo.uat.xxx.com
pro.meta=http://apollo.xxx.com
``
