# 23种设计模式

所谓设计模式，是在大量的实践中，总结和理论化的优选代码结构、编程风格以及解决问题的思考方式

## 创建型模式5种

### 单例设计模式-Singleton

在整个软件系统中，某个类只能存在一个对象实例：

1. 构造器的权限为private：在类外，不能使用new创建新的对象
2. 设定一个静态变量用以指向类的唯一实例
3. 设定一个静态方法用以返回指向类唯一实例的静态变量

单例模式的优点在于，仅存在一个实例，减少了系统的性能开销；如果一个对象的产生需要较多的资源时，可以在应用启动的时候直接产生一个单例对象，永久驻留内存

一个典型应用为java.lang.Runtime

单例模式有两种实现方式：

```java
//饿汉式实现
public class A{
    //创建唯一实例
    private static A instance = new A();
    //私有化构造器
    private A(){
        
    }
    //提供公共方法返回唯一实例
    public static A getInstance(){
        return instance;
    }
}



//懒汉式实现-线程不安全版
public class A{
    //声明，不初始化
    private static A instance = null;
    //私有化构造器
    private A(){
        
    }
    //提供公共方法返回唯一实例
    public static A getInstance(){
        return instance = (instance == null ? new A() : instance);
    }
}


//懒汉式实现-线程安全解决方式一
public class A{
    //声明，不初始化
    private static A instance = null;
    //私有化构造器
    private A(){
        
    }
    //提供公共方法返回唯一实例
    public static synchronized A getInstance(){ //当前同步锁为A.class
        return instance = (instance == null ? new A() : instance);
    }
}
//懒汉式实现-线程安全解决方式一的第二种写法
//效率较差
public class A{
    //声明，不初始化
    private static A instance = null;
    //私有化构造器
    private A(){
        
    }
    //提供公共方法返回唯一实例
    public static A getInstance(){
        synchronized(A.class){ //显式设定同步锁
            return instance = (instance == null ? new A() : instance);
        }
    }
}

//懒汉式实现-线程安全解决方式二
//效率较高
public class A{
    //声明，不初始化
    private static A instance = null;
    //私有化构造器
    private A(){
        
    }
    //提供公共方法返回唯一实例
    public static A getInstance(){
        if(instance == null){
            synchronized(A.class){ //显式设定同步锁
            	return instance = (instance == null ? new A() : instance);
        	}
        }
        return instance;
    }
}
```

这两种方式的区别在于：

1. 懒汉式仅在需要的时候才创建实例，延迟对象的创建；饿汉式因为一开始就创建好了实例，会导致对象的加载时间过长
2. 饿汉式天然是线程安全的，懒汉式天然不是线程安全的

单例模式的主要应用场景：

1. 网站计数器
2. 应用程序日志应用，共享的日志文件一直处于打开状态，因此只能有一个实例去操作
3. 数据库连接池
4. 读取配置文件的类，没有必要每次使用配置文件数据的时候都生成一个新对象
5. Application
6. Windows中的任务管理器
7. Windows中的回收站

### 工厂方法模式

创建者与调用者分离，将创建对象的具体过程屏蔽、隔离起来，以提高灵活性

### 抽象工厂模式



## 结构型模式7种

### 代理模式-Proxy

为一个对象提供一种代理，以控制对这个对象的访问

1. 安全代理：屏蔽对真实角色的直接访问
2. 远程代理：通过代理类处理远程方法调用
3. 延迟加载：优先加载轻量级对象

静态代理&动态代理

## 行为型模式11种

### 模板方法设计模式-TemplateMethod

抽象类作为多个子类的通用模板，子类在抽象类的基础上进行拓展、改造，但子类总体上会保留抽象类的行为方式

应用于：当功能内部一部分的实现是确定的、但一部分的实现是不确定的，这时候将不确定的部分暴露出去，交给子类实现

1. 数据库访问
2. JUnit
3. JavaWeb中Servlet
4. Hibernate中的模板程序
5. Spring中的JDBC Template、Hibernate Template

## MVC模式

用于划分不同类的功能

Model：模型层，处理主要数据，数据模型。数据对象的封装、数据库的操作、数据库

View：视图层，显示数据相关的工具类、自定义的UI

Controller：处理业务逻辑，应用界面相关、服务相关的、抽取的基类等