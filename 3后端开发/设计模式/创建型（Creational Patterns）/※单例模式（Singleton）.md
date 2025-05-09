# 单例模式

* **功能**：确保一个类只有一个实例，并提供一个全局访问点。
* **应用场景**：配置文件的读取、数据库连接池等。

## 1.饿汉式

```JAVA
/**
 * @author YC
 * 饿汉式单例模式
 * 类加载时直接创建类的对象
 */
public class ClassName {
    /**
     * 在本类创建出唯一的实例
     */
    private ClassName INSTANCE = new ClassName();
    /**
     * 构造函数私有化
     */
    private ClassName() {

    }

    /**
     * 全局唯一的对外获取实例的方法
     * @return ConfigManager唯一实例
     */
    public ClassName getInstance() {
        return INSTANCE;
    }
}
```

 ## 2.懒汉式

```JAVA
/**
 * @author YC
 * 懒汉式单例模式
 * 类加载时不会创建类的对象，第一次调用getInstance()方法时才会创建对象
 * 需要注意线程安全：
 * （1）.使用synchronized关键字锁住整个方法
 * （2★）.线程安全处理：使用双重检查 + volatile的方式
 */
public class ClassName {
    /**
     * 在本类创建出唯一的实例
     */
    private volatile ClassName INSTANCE = null;
    /**
     * 构造函数私有化
     */
    private ClassName() {

    }

     //    /**
     //     * 全局唯一的对外获取实例的方法
     //     * 线程安全处理：使用synchronized关键字锁住整个方法
     //     *
     //     * @return ClassName唯一实例
     //     */
     //    public synchronized ClassName getInstance() {
     //        if (INSTANCE == null) {
     //            INSTANCE = new ClassName();
     //        }
     //        return INSTANCE;
     //    }

    /**
     * 全局唯一的对外获取实例的方法
     * 线程安全处理：使用双重检查 + volatile的方式
     * @return ClassName唯一实例
     */
    public ClassName getInstance() {
        if (INSTANCE == null) {
            synchronized (ClassName.class) {
                if (INSTANCE == null) {
                // 正常执行顺序：初始化一个ConfigManager对象 再将对象赋值给INSTANCE
                // 指令重排序（JVM优化）：将对象先赋值给INSTANCE 再将对象中的变量初始化对象 以提升速度（在声明处加入volatile关键字禁止指令重排序）
                	INSTANCE = new ClassName();
                }
            }
        }
        return INSTANCE;
    }
}
```
