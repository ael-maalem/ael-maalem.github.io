---
title: Spring Bean Scopes and Autowiring
author: Abdennacer El-Maalem
date: 2020-07-05 06:45:00 +0800
tags:
  - Spring
  - Spring Framework
  - JAVA
toc: false
---

![Autowiring in Spring](../../../public/assets/img/sample/Autowiring/Autowiring.png)

## Introduction :

Greeting 😄😄😄

Today we will continue about the concepts of Spring Framework concerning Spring Bean
`Scope` that allows us to have more granular control of the bean instances creation and
the annotation `@AutoWiring` allows the Spring container to automatically resolve
dependencies between collaborating beans.

## Spring Scope

The scope of a bean defines the life cycle and visibility of that bean in the contexts
in which it is used.

There are five scopes that spring provides to configure a bean :

1. **Singleton**:

   Each time we ask for an istance of that bean from the application context ,
   it's returning the same bean for us .

   `AppConfig.class`:

   ```java
   @Configuration
   public class AppConfig {

       @Bean(name="speakerService")
       @Scope(value = BeanDefinition.SCOPE_SINGLETON) //by default
       public SpeakerService getSpeakerService(){
           SpeakerServiceImpl service=new SpeakerServiceImpl(getSpeakerRepository());
           return service;
       }

       @Bean(name="speakerRepository")
       public SpeakerRepository getSpeakerRepository(){
           return new HibernateSpeakerRepositoryImpl();
       }
   }
   ```

   `Application.class`:

   ```java
   public class Application {
       public static void main(String[] args) {

           ApplicationContext applicationContext=new AnnotationConfigApplicationContext(AppConfig.class);// when this line run ,it's going to create a basic registry with 2 beans (speakerService bean ,speakerRepository bean)

           SpeakerService service1= applicationContext.getBean("speakerService",SpeakerService.class);
           SpeakerService service2= applicationContext.getBean("speakerService",SpeakerService.class);

           System.out.println(service1);
           System.out.println(service1.findAll().get(0).getFirstName());
           System.out.println(service2);
       }

   }
   ```

   `Output`:

   ![Output](../../../public/assets/img/sample/Autowiring/output-1.png)

   As a result, we have the same memory address for both objects: `Service1`, `Service2`.

   > `Singleton` scope is the default scope of Spring.

   > The difference between Singleton in java and in spring:

   ![Singleton Java Vs Singleton Spring](../../../public/assets/img/sample/Autowiring/Singleton-Java-Vs-Spring.png)

2. **Prototype**:

   It gives us a unique bean per request or reference(`getbean()`) and it is the opposite of singleton scope.

   `AppConfig.class`:

   ```java
     @Configuration
      public class AppConfig {

        @Bean(name="speakerService")
        @Scope(value = BeanDefinition.SCOPE_Prototype)
        public SpeakerService getSpeakerService(){
            SpeakerServiceImpl service=new SpeakerServiceImpl(getSpeakerRepository());
            return service;
        }

        @Bean(name="speakerRepository")
        public SpeakerRepository getSpeakerRepository(){
            return new HibernateSpeakerRepositoryImpl();
        }
    }
   ```

   `Output`:

   ![Output](../../../public/assets/img/sample/Autowiring/output-2.png)

   As a result, The object `Service1` have different memory address with the object `Service2`.

3. **Session**:

   Return a new bean per HTTP request.

4. **Request**:

   Return a single bean per HTTP. for example when the user session is alive for 10 or 30 min.

5. **Global**:

   Return a single bean per application. So once to access it, it’s alive for the duration of that application.

> The three last scopes: Session,Request and Global will be introduced in the future blog when we talk about Spring MVC.

### Autowiring

The annotation `Autowiring` allows the Spring container to automatically resolve dependencies
between collaborating beans by inspecting the beans that have been defined.
And it is a great technique used to reduce the wiring up and configuration of code(**convention-over-configuration**).

`Autowiring` is a great feature that allows spring to get the reference objects
by name or by type(Not to worry by the terms, we will explain in a while).

To `autowiring` using **Java configuration** we just simply add:

- `@ComponentScan({“com.elmmalem”})` into configuration file.

  `AppConfig.class`:

  ```java
   @Configuration
   @ComponentScan({"com.elmaalem"})
   public class AppConfig {


   }
  ```

- `@Bean` **by Name** or **by Type**(instance type ).

  `AppConfig.class`:

  ```java
      @Configuration
          public class AppConfig {

              @Bean(name="speakerService")
              public SpeakerService getSpeakerService(){
                  SpeakerServiceImpl service=new SpeakerServiceImpl(getSpeakerRepository());
                  return service;
              }

              @Bean(name="speakerRepository")
              public SpeakerRepository getSpeakerRepository(){
                  return new HibernateSpeakerRepositoryImpl();
              }
          }
  ```

  > `Autowiring` by Type cannot happen if there are two or more variables of the same datatype. i.e. If `SpeakerServiceImpl` class contains two variables of type `SpeakerRepository`, `Autowiring` by Type does not work.

### Conclusion

That was all for this blog, I hope you enjoy it and you understood two techniques
that we have touched on. Therefore, In the next blog, we will explain spring
configuration using java with `stereotype annotations` and `advanced bean configuration`.

Thank you for reading! If you enjoyed it, please upvote 👍
it and don’t forget to share! 👍🤙
