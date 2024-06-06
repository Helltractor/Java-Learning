# Spring Boot

## @SpringBootApplication

`@SpringBootApplication` �� Spring Boot ��һ�����õ�ע�⣬ͨ�����������ϣ��Ա�ʶ����һ�� Spring Boot Ӧ�á�����һ�����ע�⣬�����˶������ע�⣬��ʹ�� Spring Boot Ӧ�õ����ø��Ӽ򵥺ͱ�ݡ������Ƕ����ע�⼰�������ע�����ϸ������

### @SpringBootConfiguration

���ע���� `@Configuration` ���ػ��汾����Ҫ���� Spring Boot ��Ŀ�У�����������һ�������࣬������������ bean��

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
}
```

### @EnableAutoConfiguration

���ע������ Spring Boot ���Զ����û��ƣ�������Ŀ�е������������Զ����� Spring Ӧ�á�

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
    // �ų�ĳЩ�Զ�������
    Class<?>[] exclude() default {};

    // �ų�ĳЩ�Զ������������
    String[] excludeName() default {};
}
```

### @ComponentScan

���ע���������ɨ�裬ɨ���������ڰ������Ӱ��е������

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Repeatable(ComponentScans.class)
public @interface ComponentScan {
    // Ҫɨ��Ļ�����
    @AliasFor("basePackages")
    String[] value() default {};

    // Ҫɨ��Ļ�����
    @AliasFor("value")
    String[] basePackages() default {};

    // Ҫɨ��Ļ�������
    Class<?>[] basePackageClasses() default {};

    // ָ����������bean���Ƶ�������
    Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;

    // ��������ʡ��...
}
```

### @SpringBootApplication����ϸ����

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
        type = FilterType.CUSTOM,
        classes = {TypeExcludeFilter.class}
    ), @Filter(
        type = FilterType.CUSTOM,
        classes = {AutoConfigurationExcludeFilter.class}
    )}
)
public @interface SpringBootApplication {
    // ָ��Ҫ�ų����Զ�������
    @AliasFor(annotation = EnableAutoConfiguration.class)
    Class<?>[] exclude() default {};

    // ָ��Ҫ�ų����Զ�����������
    @AliasFor(annotation = EnableAutoConfiguration.class)
    String[] excludeName() default {};

    // ָ��Ҫɨ��Ļ�����
    @AliasFor(annotation = ComponentScan.class, attribute = "basePackages")
    String[] scanBasePackages() default {};

    // ָ��Ҫɨ��Ļ�������
    @AliasFor(annotation = ComponentScan.class, attribute = "basePackageClasses")
    Class<?>[] scanBasePackageClasses() default {};

    // ָ����������bean���Ƶ�������
    @AliasFor(annotation = ComponentScan.class, attribute = "nameGenerator")
    Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;

    // ָ���������Ƿ������bean����
    @AliasFor(annotation = Configuration.class)
    boolean proxyBeanMethods() default true;
}
```

### �ܽ�

* `@SpringBootApplication` ��һ�����ע�⣬���� `@SpringBootConfiguration`��`@EnableAutoConfiguration` �� `@ComponentScan`��
* ͨ�����ע�⣬Spring Boot Ӧ���ܹ��Զ�ɨ���������Ŀ�е�����������������Զ��������ã��Ӷ������˿����ߵ����ù�������
* ��ע��ͨ��ʹ�� `@AliasFor` �����Դ����������ע�⣬�������á�

ʹ�� `@SpringBootApplication` ���Դ��� Spring Boot Ӧ�õ����������ù��̣�������ֻ���������ϱ�ע���ע�⼴����ɴ󲿷����ù�����

## SpringApplication.run()

��δ���չʾ��Spring BootӦ�õ��������̡�`SpringApplication`���`run`������Spring Boot�����ĺ��ģ��������ʼ�������ú�����SpringӦ�á�������ϸ�������������ÿһ�����԰������õ����Spring BootӦ�õ��������̡�

### `run` ��������

```java 17
public ConfigurableApplicationContext run(String... args) {
    // 1. ���������������ڼ�¼����ʱ��������Ϣ
    Startup startup = SpringApplication.Startup.create();
    
    // 2. ��������˹رչ��ӣ���ע��رչ���
    if (this.registerShutdownHook) {
        shutdownHook.enableShutdownHookAddition();
    }

    // 3. �������������ģ�Ĭ��ʵ��Ϊ�������ģ��������ڴ洢һЩ��ʱ����
    DefaultBootstrapContext bootstrapContext = this.createBootstrapContext();
    ConfigurableApplicationContext context = null;
    
    // 4. ����headless����
    this.configureHeadlessProperty();
    
    // 5. ��ȡ�����������������������¼�
    SpringApplicationRunListeners listeners = this.getRunListeners(args);
    listeners.starting(bootstrapContext, this.mainApplicationClass);

    Throwable ex;
    try {
        // 6. ����Ӧ�ó������
        ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
        
        // 7. ׼������������ϵͳ����������Ӧ�����õ�
        ConfigurableEnvironment environment = this.prepareEnvironment(listeners, bootstrapContext, applicationArguments);
        
        // 8. ��ӡBanner
        Banner printedBanner = this.printBanner(environment);
        
        // 9. ����Ӧ��������
        context = this.createApplicationContext();
        context.setApplicationStartup(this.applicationStartup);
        
        // 10. ׼�������ģ������������������������ȵ�ע��
        this.prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
        
        // 11. ˢ�������ģ���ʼ������Spring�����е�bean
        this.refreshContext(context);
        
        // 12. ������ˢ�º�Ĳ���
        this.afterRefresh(context, applicationArguments);
        
        // 13. ���������¼������ɵ�ʱ��
        startup.started();
        
        // 14. ��¼������Ϣ��־
        if (this.logStartupInfo) {
            (new StartupInfoLogger(this.mainApplicationClass)).logStarted(this.getApplicationLog(), startup);
        }

        // 15. ������������������¼�
        listeners.started(context, startup.timeTakenToStarted());
        
        // 16. ��������Ӧ�ó���������
        this.callRunners(context, applicationArguments);
    } catch (Throwable var10) {
        ex = var10;
        // 17. ��������ʱʧ�ܣ������쳣������
        throw this.handleRunFailure(context, ex, listeners);
    }

    try {
        // 18. ����������������У�����Ӧ��׼�������¼�
        if (context.isRunning()) {
            listeners.ready(context, startup.ready());
        }

        // 19. ����Ӧ��������
        return context;
    } catch (Throwable var9) {
        ex = var9;
        // 20. ����׼�������׶ε�ʧ��
        throw this.handleRunFailure(context, ex, (SpringApplicationRunListeners)null);
    }
}
```

### ��������ϸ����

1. **������������**��������¼����ʱ��������Ϣ�����������������ܡ�
2. **ע��رչ���**����JVM�ر�ʱִ��һЩ���������
3. **��������������**�����ڴ洢������������Ҫ����ʱ���ݡ�
4. **����headless����**������headlessģʽ��ͨ����û����ʾ�豸�Ļ���������ʱʹ�á�
5. **��ȡ����������**���������������ڼ���Spring BootӦ�õ����������¼���
6. **����Ӧ�ó������**���������ݸ�Ӧ�õ������в�����
7. **׼������**��׼��SpringӦ�õ����л�����������ȡϵͳ���Ժ������ļ���
8. **��ӡBanner**���ڿ���̨��ӡBanner��Ϣ��ͨ����Ӧ�õĻ�ӭ��Ϣ��
9. **����Ӧ��������**��Spring��IOC����������Ӧ�õ�bean��
10. **׼��������**������������������������ע�뵽Ӧ���������С�
11. **ˢ��������**����ʼ������Spring�����е�bean��ִ������ע�롣
12. **������ˢ�º�Ĳ���**����������ˢ�º�ִ��һЩ���ƵĲ�����
13. **��¼����ʱ��**����¼������ɵ�ʱ�䣬�������ܷ�����
14. **��¼������Ϣ��־**����������Ϣ��¼����־�С�
15. **������������������¼�**��֪ͨ���м�����������������ɡ�
16. **����Ӧ�ó���������**��ִ��ʵ����`ApplicationRunner`��`CommandLineRunner`�ӿڵ�bean��
17. **��������ʱʧ��**���������������е��쳣������
18. **����Ӧ��׼�������¼�**��֪ͨ���м�����Ӧ����׼��������
19. **����Ӧ��������**�������������Ӧ�������ġ�
20. **����׼�������׶ε�ʧ��**������׼�����������е��쳣������

### �ܽ�

ͨ����Щ���裬Spring Boot������ʱ����˻���׼���������ĳ�ʼ����bean���غ�����ע�롢������֪ͨ��һϵ�в�����ȷ��Ӧ���ܹ���ȷ���������С�������������з����쳣����ͨ������ʹ����쳣��ȷ��Ӧ���ܹ����������ŵ�Ӧ������ʧ�ܵ������

## Spring Boot ��������

1. **���� `SpringApplication` ����**
* ʹ�ô�������ࣨͨ������ `@SpringBootApplication` ע����ࣩ����һ�� `SpringApplication` ʵ����

2. **����Ĭ������**
* ����һЩĬ�����ԣ����� banner ģʽ���Ƿ���ӹرչ��ӵȡ�

3. **׼�� `SpringApplicationRunListeners`**
* ��ʼ�����������е� `SpringApplicationRunListeners`������Ӧ�õ����������¼���

4. **׼������**
* ���������� `ConfigurableEnvironment`��������ȡϵͳ���ԡ����������������ļ��ȡ�
* ���� `EnvironmentPreparedEvent` �¼���

5. **��ӡ Banner**
* �ڿ���̨��ӡ Banner ��Ϣ����������� Banner����

6. **����Ӧ��������**
* ����Ӧ�����ͣ����� `ServletWebServerApplicationContext` �� `ReactiveWebServerApplicationContext`���������ʵ�Ӧ�������ġ�

7. **׼��������**
* ׼��������Ӧ�������ģ��������û���������Ӧ�ü�����������������ȡ�
* �� `ApplicationArguments` �� `BootstrapContext` ע�뵽�������С�
* ���� `ApplicationPreparedEvent` �¼���

8. **ˢ��������**
* ˢ��Ӧ�������ģ���ʼ������ Spring �����е� bean��ִ������ע�롣
* ע�����е� `CommandLineRunner` �� `ApplicationRunner`��

9. **����**
* ��������ˢ�º�ִ��һЩ���ƵĲ�������������Ƕ��ʽ�������ȡ�
* ���� `ApplicationStartedEvent` �¼���

10. **����������**
* ��������ʵ���� `CommandLineRunner` �� `ApplicationRunner` �ӿڵ� bean��

11. **׼������**
* ���� `ApplicationReadyEvent` �¼�����ʾӦ����׼��������

12. **��������ʧ��**
* ������������г����쳣�����񲢴����쳣������ `ApplicationFailedEvent` �¼���

## Reference

- [Java�����⣺Spring�е�ѭ��������������Ա������������Ӱ](https://www.cnblogs.com/marsitman/p/18195525)
- [��Spring Boot���裺��������Դ�������](https://www.cnblogs.com/java-chen-hao/p/11829056.html)
- [SpringBoot3.x��spring.factories���ܱ��Ƴ��Ľ������](https://www.cnblogs.com/throwable/p/16950353.html)
- ~~[��Spring Boot�Զ�װ��ԭ����һƪ�͹���!��](https://mp.weixin.qq.com/s/f6oED1hbiWat_0HOwxgfnA)~~