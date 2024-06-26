# Singleton（单例模式）

## introduction

单例模式是一种常用的软件设计模式，它保证一个类只有一个实例，并提供一个全局访问点。这种模式通常用于需要频繁创建和销毁的对象，以减少系统资源的消耗，提高系统效率。  

- 单例模式是一种创建型设计模式，它保证一个类只有一个实例，并提供一个全局访问点。
- 单例模式的优点是可以减少内存开销，提高性能，缺点是增加了代码的复杂度。
- 单例模式在实际开发中应用广泛，例如数据库连接池、线程池、日志对象等。
- 单例模式的线程安全问题是实现时需要考虑的重要问题，如果不考虑线程安全问题，可能会导致多个实例被创建。


## implementation mode（实现方式）

单例模式的实现方式有很多种，其中最常见的是懒汉式和饿汉式。
- **懒汉式是指在第一次调用获取单例对象的方法时，才创建实例。** 懒汉式的实现方式有很多种，其中最常见的是双重检查锁和静态内部类。双重检查锁是指在同步块内进行两次null检查，以确保只有一个线程创建实例。静态内部类是指将实例化的过程放在一个静态内部类中，从而实现延迟加载。
- **饿汉式是指在类加载时就创建实例。** 饿汉式的实现方式有很多种，其中最常见的是静态常量和静态代码块。静态常量是指直接创建实例并赋值给一个静态常量，从而实现在类加载时就创建实例。静态代码块是指在静态代码块中创建实例，也是在类加载时就创建实例。

### 饿汉式

### 双重检验锁方式

双重检验锁方式实现单例模式是一种线程安全的实现方式。其原理是：第一次检查是否实例已经创建，如果尚未创建，才进行同步，这是第一重检查。然后在同步代码块中再次检查是否实例已经创建，如果尚未创建，才真正创建实例，这是第二重检查。这种方式既保证了线程安全，又比直接用synchronized关键字锁住创建实例的方法效率更高。

```Java
public class Singleton {
    // 使用volatile关键字保证了uniqueInstance变量的可见性，volatile关键字会强制将修改的值立即写入主存
    private volatile static Singleton uniqueInstance;

    // 构造函数私有化，防止外部创建实例
    private Singleton() {}

    // 获取单例对象的方法
    public static Singleton getUniqueInstance() {
        // 第一次检查实例是否已经被创建，如果尚未创建，才进入同步块
        if (uniqueInstance == null) {
            // 使用类对象作为锁，这样可以保证在多线程环境下，只有一条线程能进入同步块，从而避免实例被多次创建
            synchronized (Singleton.class) {
                // 进入同步块后，再检查一次。如果仍是null,才创建实例
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        // 返回单例对象
        return uniqueInstance;
    }
}
```

### 静态内部类方式

静态内部类方式实现单例模式是一种线程安全的实现方式。其原理是：在静态内部类中创建实例，从而实现延迟加载。当外部类加载时，静态内部类并不会被加载，只有在调用getInstance方法时，才会加载静态内部类，从而创建实例。这种方式既保证了线程安全，又实现了延迟加载。  
在这个代码中，getInstance方法是同步的，这意味着在多线程环境下，只有一条线程能够进入这个方法，从而避免了多个实例被创建。但是，这种方式的缺点是每次调用getInstance方法时都需要进行同步，这会影响性能。

```Java
public class Singleton {
    // 唯一的单例对象实例，初始时为null
    private static Singleton instance;

    // 私有的构造函数，防止外部通过new创建实例
    private Singleton() {}

    // 获取单例对象的方法
    public static synchronized Singleton getInstance() {
        // 如果单例对象还未创建，就创建一个新的实例
        if (instance == null) {
            instance = new Singleton();
        }
        // 返回单例对象
        return instance;
    }
}
```