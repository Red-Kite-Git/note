# 代理模式

* **功能**：代理模式 可以在不破坏原类代码的情况下 在之前和之后做一些事情，为其他对象提供一种代理以控制访问。
* **应用场景**：远程代理、虚拟代理、安全代理等。

## 1.静态代理

需要自己创建代理类

Subject.java ▼

```java
public interface Subject {
    
    /**
     * 模拟需要实现的业务功能
     */
    void request();
    
}
```

RealSubject.java ▼

```java
public class RealSubject implements Subject {
    
    @Override
    public void request() {
        System.out.println("！！！！！模拟业务功能！！！！！");
    }
    
}
```

ProxySubject.java ▼

```java
public class ProxySubject implements Subject {

    private final Subject subject;

    public ProxySubject(RealSubject realSubject) {
        this.subject = realSubject;
    }

    @Override
    public void request() {
        before();
        subject.request();
        after();
    }

    private void before() {
        System.out.println("调用request方法前做一些额外的事情");
    }


    private void after() {
        System.out.println("调用request方法后做一些额外的事情");
    }


    public static void main(String[] args) {
        ProxySubject proxySubject = new ProxySubject(new RealSubject());
        proxySubject.request();
    }

}
```



## 2.动态代理

自动创建代理类，JDK动态代理、Cglib动态代理

### <1>JDK动态代理

Role.java ▼

```java
/*
 * 模拟实体
 */
public class Role {
}
```

RoleService ▼

```java
/*
 * 模拟业务接口
 */
public interface RoleService {

    int addRole(Role role);

}
```

RoleServiceImpl ▼

```Java
/*
 * 模拟业务接口实现
 */
public class RoleServiceImpl implements RoleService{

    @Override
    public int addRole(Role role) {
        return 0;
    }

}
```

MethodInvocation.java ▼

```java
/*
 * 通过反射技术，在目标方法（RoleServiceImpl类中的addRole方法）调用前后记录日志
 */
public class MethodInvocation implements InvocationHandler {

    private final Logger LOGGER = Logger.getLogger(MethodInvocation.class);

    private final Object target;

    public MethodInvocation(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        LOGGER.info("在目标方法之前记录日志功能");

        //调用目标对象中的方法，target目标对象 args方法的参数 method目标方法
        Object result = method.invoke(target, args);

        LOGGER.info("在目标方法之后记录日志功能");

        return result;

    }
}
```

ProxyTest.java ▼

```java
/*
 * 测试类
 */
public class ProxyTest {

    public static void main(String[] args) {

        RoleService roleService = (RoleService) Proxy.newProxyInstance(
                RoleServiceImpl.class.getClassLoader(),
                RoleServiceImpl.class.getInterfaces(),
                new MethodInvocation(new RoleServiceImpl())
        );

        roleService.addRole(new Role());

    }

}
```

### <2>Cglib动态代理