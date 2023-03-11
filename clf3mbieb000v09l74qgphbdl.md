---
title: "How To Do @Async in Spring Boot"
seoDescription: "Learn asynchronous programming in your Spring Boot application with this comprehensive guide. Discover how to use Spring's built-in @Async annotation"
datePublished: Sat Mar 11 2023 07:01:08 GMT+0000 (Coordinated Universal Time)
cuid: clf3mbieb000v09l74qgphbdl
slug: how-to-do-async-in-spring-boot
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/K6cC1D-_k_g/upload/a608bb63ca356f6b6eb4ba60704e126d.jpeg
tags: asynchronous, async, springboot

---

Asynchronous programming has become increasingly popular in recent years as it allows developers to write non-blocking, high-performance code. In the Spring Boot framework, asynchronous programming is achieved using a combination of Java's CompletableFuture and Spring's own annotations.

Let's take a closer look at how async works in Spring Boot.

1. Defining Async Methods
    

The first step in implementing asynchronous programming in Spring Boot is to define methods that will execute asynchronously. These methods should return a CompletableFuture object, which is a special type of object that represents an asynchronous operation that may or may not have completed yet.

These methods can also have a return type of void

Here's an example of an async method:

```java
@Service
public class MyService {
 
    @Async
    public CompletableFuture<String> asyncMethod() {
        // This can be any work, like save to db, call some api, or compute a heavy operation
        logger.info("asyncMethod Called in MyService");

        BigInteger result = BigInteger.valueOf(1);
        for (int i = 2; i <= 5; i++) {
            result = result.multiply(BigInteger.valueOf(i));
        }
        return CompletableFuture.completedFuture("Result: " + result.toString());
    }
 
}
```

The method returns a CompletableFuture&lt;String&gt;, which will eventually contain a String result once the async operation is complete.

1. Configuring Async Execution
    
    Next, we need to configure Spring to execute our async methods on a separate thread pool. This can be done using the EnableAsync annotation on a configuration class:
    
    ```java
    @Configuration
    @EnableAsync
    public class AsyncConfig {
     
        @Bean(name = "myTaskExecutor")
        public Executor getAsyncExecutor() {
            ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
            executor.setCorePoolSize(4);
            executor.setMaxPoolSize(4);
            executor.setQueueCapacity(500);
            executor.setThreadNamePrefix("MyAsyncThread-");
            executor.initialize();
            return executor;
        }
     
    }
    ```
    
    In this example, we define a custom Executor bean named "myTaskExecutor" that will execute our async methods. The ThreadPoolTaskExecutor is used to create a thread pool with a core size of 4, a max size of 4, and a queue capacity of 500.
    

> Spring Boot provides a default Executor configuration for async methods, and you don't necessarily need to define your own Executor bean.
> 
> By default, Spring Boot uses the SimpleAsyncTaskExecutor, which creates a new thread for each async method invocation. While this may be sufficient for small applications, it may not be ideal for larger applications with more complex async requirements.

Invoking Async Methods

```java
@RestController
public class MyController {
 
    @Autowired
    private MyService myService;
 
    @GetMapping("/async")
    public CompletableFuture<String> asyncEndpoint() {
        return myService.asyncMethod();
    }
 
}
```

### How to ensure async is working properly

You can check the thread name from the logs in the controller and async method.

`2023-03-11T10:37:40.268+05:30 INFO 70621 --- [nio-8080-exec-1] com.example.async.demo.MyController : asyncEndpoint Called in MyController 2023-03-`

`11T10:37:40.272+05:30 INFO 70621 --- [ task-1] com.example.async.demo.MyService : asyncMethod Called in MyService` `// when spring boot default executor is used`

`2023-03-11T12:17:15.388+05:30 INFO 73114 --- [MyAsyncThread-1] com.example.async.demo.MyService : asyncMethod Called in MyService` `// when myTaskExecutor is used`

`nio-8080-exec-1` and `task-1/MyAsyncThread-1` are two different thread names.

1. Handling Async Results
    
    Once our async method has completed, we need to handle the result. This can be done using a callback function that is passed to the CompletableFuture's thenApply method:
    
    ```java
    CompletableFuture<String> futureResult = myService.asyncMethod();
    futureResult.thenApply(result -> {
        // handle the result here
        return result;
    });
    ```
    

### How to handle exceptions in async methods?

If you define your own Executor, you should also define an UncaughtExceptionHandler to handle any uncaught exceptions that may occur in your async methods.

An UncaughtExceptionHandler is a callback function that is invoked when a thread encounters an uncaught exception. By default, if an async method encounters an uncaught exception, the exception will be logged but otherwise ignored. Defining an UncaughtExceptionHandler allows you to handle these exceptions in a more appropriate way.

Here's an example of defining an UncaughtExceptionHandler for a custom Executor:

```java
@Configuration
@EnableAsync
public class AsyncConfig implements AsyncConfigurer {

    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(4);
        executor.setMaxPoolSize(4);
        executor.setQueueCapacity(500);
        executor.setThreadNamePrefix("MyAsyncThread-");
        executor.initialize();
        return executor;
    }

    @Override
    public AsyncUncaughtExceptionHandler getAsyncUncaughtExceptionHandler() {
        return new SimpleAsyncUncaughtExceptionHandler();
    }

}
```

If you look closely, we have changed the structure of the AsyncConfig class, earlier it was returning a bean of type Executor, Now it `implements AsyncConfigurer`

### Using AsyncConfigurer class to configure executor

Implementing the `AsyncConfigurer` interface in a Spring Boot application allows you to customize the configuration of the async execution of your methods.

By implementing `AsyncConfigurer`, you can provide your own `Executor` bean and specify various configuration options for it, such as the thread pool size, queue capacity, and thread name prefix. You can also set a task decorator to customize the behavior of the async methods.

Additionally, implementing `AsyncConfigurer` allows you to define a custom `AsyncUncaughtExceptionHandler` to handle any exceptions that may occur in your async methods.

We define a custom `AsyncUncaughtExceptionHandler` bean by implementing the `getAsyncUncaughtExceptionHandler()` method. This allows us to handle any uncaught exceptions that may occur in our async methods. In this example, we return an instance of `SimpleAsyncUncaughtExceptionHandler`, which is a simple class to log exceptions.

Conclusion

Async programming is a powerful technique for improving the performance and responsiveness of our applications. In Spring Boot, async programming is made easy by the use of CompletableFuture and Spring's own annotations. By following the steps outlined in this article, you can easily implement async methods in your own Spring Boot applications.

Check the code for this example: [https://github.com/nkalra0123/async\_spring\_boot](https://github.com/nkalra0123/async_spring_boot)

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.