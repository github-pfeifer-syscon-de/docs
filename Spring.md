
# Spring

a rich application framework. As i'am not a friend of all the maven stuff here some (minimal dependencies) steps:

## Simple bean

### Libraries

Download from:

[https://repo.spring.io/release/org/springframework/spring/4.3.9.RELEASE/spring-framework-4.3.9.RELEASE-dist.zip Spring repo](https://repo.spring.io/release/org/springframework/spring/4.3.9.RELEASE/spring-framework-4.3.9.RELEASE-dist.zip Spring repo.md)


Add to libraries (spring-core):

* spring-beans
* spring-core
* spring-context
* spring-context-support
* spring-expression

Add to libraries (spring-aop):

* spring-aop

### Declare

```
@Configuration
public class AppConfig {
   @Bean  @Scope(value = ConfigurableBeanFactory.SCOPE_SINGLETON)
   public CarService carService() {
       return new CarServiceImpl();
   }
}
```

### Access

```
ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
if (carService == null) {
   carService = ctx.getBean(CarService.class);
}
```

## Jpa Dependencies

The correct naming woud be "spring-data jpa bridge" ...

* spring-data-jpa-2.3.0
* spring-aop-4.3.1
* spring-data-commons-2.3.0
* spring-jdbc-5.2.6
* spring-orm-5.2.6
* spring-tx-5.2.6

## Links web-development

[Spring getting started](https://spring.io/guides#getting-started-guides)

[Zk Mvvm Java](https://www.zkoss.org/wiki/Small%20Talks/2012/August/MVVM%20In%20Java)

[Angular Spring](http://websystique.com/springmvc/spring-mvc-4-angularjs-example/)
