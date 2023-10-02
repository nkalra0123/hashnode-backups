---
title: "Difference between Spring Boot  and Spring Framework"
datePublished: Mon Oct 02 2023 07:15:13 GMT+0000 (Coordinated Universal Time)
cuid: cln8k39m4000409jnc8ntckgw
slug: difference-between-spring-boot-and-spring-framework
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/vpOeXr5wmR4/upload/7f347bca3bd25d53ddc03c1dd792e762.jpeg
tags: springboot, spring-framework

---

Spring Universe actually consists of 21 different, active [projects](https://spring.io/projects).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696182826407/0c3f28e8-07a0-49f8-8e7d-a312f3fcecd4.png align="center")

Spring boot and Spring Framework both are part of this list.

If you started programming with Spring in the last couple of years, there is a very high chance that you went *directly* to [Spring Boot](https://spring.io/guides/gs/spring-boot/)

### **Spring Framework: The Foundation**

At its core, the Spring Framework is a comprehensive and modular framework for Java development. It provides solutions for a multitude of concerns, including but not limited to data access, transaction management, messaging, and more. The Spring Framework's Inversion of Control (IoC) container is a key feature, facilitating the management and configuration of Java objects, known as beans.

**Key Aspects of Spring Framework:**

1. **IoC Container:** Manages beans and their dependencies.
    
2. **Modules:** Offers various modules for different functionalities (e.g., Spring MVC for web applications, Spring Data for data access).
    
3. **Aspect-Oriented Programming (AOP):** Enables modularization of cross-cutting concerns.
    
4. **Enterprise Services:** Provides support for transaction management, messaging, and more.
    

### **Spring Boot: Streamlining Development**

While the Spring Framework provides a robust foundation, Spring Boot takes the development experience to the next level. Spring Boot is not a replacement for the Spring Framework; rather, it is a project within the Spring ecosystem that simplifies the process of building production-ready applications.

**Key Features of Spring Boot:**

1. **Convention over Configuration:** Minimizes the need for extensive configuration through sensible defaults.
    
2. **Embedded Servers:** Includes embedded servers like Tomcat or Jetty, reducing the need for external server configuration.
    
3. **Auto-Configuration:** Automatically configures application components based on project dependencies.
    
4. **Spring Boot Starters:** Pre-configured templates for common use cases, making it easy to kickstart projects.
    
5. **Production-Ready Defaults:** Includes features like health checks, metrics, and externalized configuration for building robust applications.
    

The Spring Framework remains the bedrock of the Spring ecosystem, offering a comprehensive set of tools and modules.

Spring Boot acts as a facilitator, streamlining development and simplifying the creation of production-ready applications.

Spring Boot builds upon the foundations laid by Spring Framework and Spring MVC.

For instance, in the Spring Framework, you can handle .properties files and create JSON REST controllers using @PropertySource annotations and the Web MVC framework, respectively. However, the challenge lies in explicitly instructing Spring on where to locate these properties and configuring its web framework, such as enabling JSON support.

Enter Spring Boot, which simplifies this process by taking care of these configuration details for you. Consider this:

1. It autonomously scans predefined locations for [application.properties](http://application.properties) files, ensuring seamless property file ingestion.
    
2. It seamlessly initializes an embedded Tomcat, allowing you to promptly witness the outcomes of your @RestControllers and kickstart web application development.
    
3. It automatically configures the environment for effortless JSON communication, sparing you the hassle of meticulous Maven/Gradle dependencies. All of this is achieved by executing the main method within a Java class adorned with the @SpringBootApplication annotation.
    

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.