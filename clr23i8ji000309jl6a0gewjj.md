---
title: "Exploring the Magic of Virtual Threads in Java"
seoTitle: "Virtual threads in Java"
seoDescription: "Java Virtual Threads Unveiled: A Deep Dive into Project Loom's Revolutionary Concurrency"
datePublished: Sat Jan 06 2024 13:23:11 GMT+0000 (Coordinated Universal Time)
cuid: clr23i8ji000309jl6a0gewjj
slug: exploring-the-magic-of-virtual-threads-in-java
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Nh6NsnqYVsI/upload/dd1bfc5570af68c86e3158a74fdbba37.jpeg
tags: java, virtualthreads

---

In the ever-evolving landscape of Java development, Project Loom has emerged as a game-changer, introducing a revolutionary concept known as virtual threads. In this blog post, we'll dive deep into the world of virtual threads, exploring their capabilities, benefits, and the underlying magic that makes them a powerful tool for developers.

## **Understanding the Need for Virtual Threads**

The traditional approach to threading in Java involves using platform threads, which can be resource-intensive and challenging to manage. With the advent of Project Loom and virtual threads, developers now have a more efficient alternative that promises to simplify concurrent programming.

The removal of the involvement of the OS in the lifecycle of a virtual thread is what removes the scalability bottleneck. Large-scale JVM applications can cope with having millions or even billions of objects, so why should they be restricted to just a few thousand OS-schedulable objects (which is one way to think about what a thread is)?

## **What are Virtual Threads?**

Virtual threads, as introduced by Java Enhancement Proposal (JEP) 425, are lightweight threads that run on top of platform threads. Unlike traditional threads, virtual threads are inexpensive to create and manage, making them an attractive option for concurrent programming tasks.

## **How to create virtual Threads**

```bash
  public static Thread makeThread(Runnable runnable) {
        return Thread.ofVirtual().start(runnable);
    }
```

This code calls the `ofVirtual()` method on the Thread class to explicitly create a virtual thread that will execute the `Runnable`. We can use `Thread.ofPlatform().start(runnable);` to OS-schedulable thread object.

## **Breaking Free from Reactive Code:**

One of the key advantages of virtual threads is their ability to make blocking threads cost-effective. With JDK 19, blocking a virtual thread becomes significantly cheaper compared to traditional platform threads. This means developers can revert to writing more readable and testable blocking synchronous code without the drawbacks of traditional blocking.

This means that rather than needing to learn a completely new programming style (such as the continuation-passing style or the [promise/future approach](https://en.wikipedia.org/wiki/Futures_and_promises) or callbacks), the Project Loom runtime keeps the same programming model you know from today’s threads for virtual threads. In other words, virtual threads are simply threads, at least as far as the programmer is concerned.

Virtual threads are *preemptive* because the user code does not need to explicitly yield control of the CPU. Scheduling points are up to the virtual scheduler and the JDK. Developers must make no assumptions on when yields happen, because that is purely an implementation detail.

When your computer is handling multiple tasks at once, it's like juggling different threads. These threads are chunks of code that the computer executes. Imagine each thread having a turn to use the CPU, and when its turn is up, the computer switches to another thread.

This process, called time-sharing, has been around for a while, even back when computers had just one brain (processing core). Now, enter virtual threads. They're a newer way of handling tasks, and unlike regular threads, they don't rely on time slices.

Regular threads are like taking turns on a timer, but virtual threads are more flexible. Instead of waiting for a timer to go off, virtual threads are smart—they automatically pause when they're waiting for something, like input/output (I/O) operations. It's like they voluntarily step aside when they don't have anything to do.

Virtual threads handle the pause-and-wait situations on their own, thanks to the magic happening behind the scenes.

So, in a nutshell, Project Loom lets Java developers write code in a straightforward way, just like they're used to, without diving into the nitty-gritty details of thread management. And it keeps things compatible with debugging tools and profilers, making everyone's life a bit simpler.

To try virtual threads, download JDK 21, and create and run virtual threads.

If you are using Spring Boot 3.2, enable it with `spring.threads.virtual.enabled=true`

[All together now: Spring Boot 3.2, GraalVM native images, Java 21, and virtual threads with Project Loom](https://spring.io/blog/2023/09/09/all-together-now-spring-boot-3-2-graalvm-native-images-java-21-and-virtual/)

### **Using *Executors.newVirtualThreadPerTaskExecutor()***

This method **creates one new virtual thread per task**. The number of threads created by the *Executor* is unbounded.

In the following example, we are submitting 10,000 tasks and waiting for all of them to complete. The code will create 10,000 virtual threads to complete these 10,000 tasks.

```java
try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
            for (int i = 0; i < 10_000; i++) {
                executor.submit(() -> {
                    System.out.println("Hello virtual thread!");
                    try {
                        Thread.sleep(Duration.ofSeconds(1));
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                });
            }
        }
```

## **Best Practices to Follow**

**DO NOT Pool the Virtual Threads**

Java thread pool was designed to avoid the overhead of creating new OS threads because creating them was a costly operation. But creating virtual threads is not expensive, so, there is never a need to pool them. It is advised to create a new virtual thread everytime we need one.

**Use *ReentrantLock* instead of *Synchronized* Blocks**

There are two specific scenarios in which a virtual thread can block the platform thread (called **pinning of OS threads**).

* When it executes code inside a synchronized block or method, or
    
* When it executes a *native method* or a *foreign function*.
    

Such `synchronized` block does not make the application incorrect, but it limits the scalability of the application similar to platform threads.

As a best practice, if a method is used very frequently and it uses a *synchronized* block then consider replacing it with the *ReentrantLock* mechanism.

```java
public synchronized void method() {
	try {
	 	// ... access resource
	} finally {
	 	//
	}
}
```

use `ReentrantLock` like this:

```java
private final ReentrantLock lock = new ReentrantLock();

public void method() {
	lock.lock();  // block until condition holds
	try {
	 	// ... access resource
	} finally {
	 	lock.unlock();
	}
}
```

## **Conclusion**

Traditional Java threads have served very well for a long time. With the growing demand of scalability and high throughput in the world of microservices, virtual threads will prove a milestone feature in Java.

With virtual thread, a program can handle millions of threads with a small amount of physical memory and computing resources, otherwise not possible with traditional platform threads.