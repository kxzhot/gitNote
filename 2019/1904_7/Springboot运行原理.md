## Springboot运行原理
我们从 springboot项目的的启动类中可以看到最核心的两行代码：
＠SpringBootapplication和 Springapplication.run方法。
#### 在＠SpringBootapplication的内部包含了3个注解
- 	@Configuration
- 	@Enableautoconfiguration
- 	@Componentscan
@Configuration是基于 Javaconfig:形式的 Spring loc容器的配置类，可以把它看成xml配置文件中的 beans标签。
      ＠Configuration写到类上面，在类中的方法上如果写了＠Bean注解，那么它的返回值将作为一个bean注册到 Spring的oC容器，方法名默认作为bean的id
＠Componentscan这个注解对应XML配置中的context: component-scan元素，说白了它的作用就是自动扫描并加载符合条件的组件比如＠Component和＠Service等或者bean定义，
   最终将这些bean定义加载到loC容器中我们可以通过 basepackages来指定＠Componentscan自动扫描的范围，如果不指定，
   则默认 Spring框架实现会从声明＠Componentscan所在类的package进行扫描。
    这也是 Springboot f的启动类最好是放在 root package下的原因
＠Enableautoconfiquration这个注解是借助＠Import的帮助，将所有符合自动配置条件的bean定义加载到loC容器中