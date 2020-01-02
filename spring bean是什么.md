Spring有跟多概念，其中最基本的一个就是bean，那到底spring bean是什么?
Bean是Spring框架中最核心的两个概念之一（另一个是面向切面编程AOP）。
是否正确理解 Bean 对于掌握和高效使用 Spring 框架至关重要。
遗憾的是，网上不计其数的文章，却没有简单而清晰的解释。
那么，Spring bean 到底是什么？
接下来我们用图文方式来解析这一个概念。

## 1 定义
Spring 官方文档对 bean 的解释是：
```
In Spring, the objects that form the backbone of your application 
and that are managed by the Spring IoC container are called beans. 
A bean is an object that is instantiated, assembled, and otherwise 
managed by a Spring IoC container.
```
翻译过来就是：
```
在 Spring 中，构成应用程序主干并由Spring IoC容器管理的对象称为
bean。bean是一个由Spring IoC容器实例化、组装和管理的对象。
```
概念简单明了，我们提取处关键的信息：

1. bean是对象，一个或者多个不限定
2. bean由Spring中一个叫IoC的东西管理
3. 我们的应用程序由一个个bean构成

第1和3好理解，那么IoC又是什么东西？

## 2 控制反转（IoC）
控制反转英文全称：Inversion of Control，简称就是IoC。

控制反转通过依赖注入（DI）方式实现对象之间的松耦合关系。

程序运行时，依赖对象由【辅助程序】动态生成并注入到被依赖对象中，动态绑定两者的使用关系。

Spring IoC容器就是这样的辅助程序，它负责对象的生成和依赖的注入，让后在交由我们使用。

简而言之，就是：IoC就是一个对象定义其依赖关系而不创建它们的过程。

这里我们可以细分为两个点。

### 2.1 私有属性保存依赖
第1点：使用私有属性保存依赖对象，并且只能通过构造函数参数传入，

构造函数的参数可以是工厂方法、保存类对象的属性、或者是工厂方法返回值。

假设我们有一个Computer类：
```
public class Computer {
    private String cpu;     // CPU型号
    private int ram;        // RAM大小，单位GB

    public Computer(String cpu, int ram) {
        this.cpu = cpu;
        this.ram = ram;
    }
}
```

