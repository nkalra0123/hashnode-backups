# Understanding the Output of JStat Command in Java

```yaml
jstat -gc 12460
 S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT
 0.0   4096.0  0.0   2591.0 75776.0  69632.0   83968.0    48156.0   60416.0 59829.4 8832.0 8565.2     10    0.036   0      0.000    0.036
```

If you ever run **jstat** on a java application, you will see some values printed like this.

In this blog, let's try to learn, what is the meaning of each value.

The following is a description of the columns in the output of the `jstat` command:

* `S0C`: The current size of the survivor space 0 in kilobytes (K).
    
* `S1C`: The current size of the survivor space 1 in kilobytes (K).
    
* `S0U`: The current usage of the survivor space 0 in kilobytes (K).
    
* `S1U`: The current usage of the survivor space 1 in kilobytes (K).
    
* `EC`: The current size of the eden space in kilobytes (K).
    
* `EU`: The current usage of the eden space in kilobytes (K).
    
* `OC`: The current size of the old space in kilobytes (K).
    
* `OU`: The current usage of the old space in kilobytes (K).
    
* `MC`: The current size of the meta space in kilobytes (K).
    
* `MU`: The current usage of the meta space in kilobytes (K).
    
* `CCSC`: The current capacity of the CodeCache in kilobytes (K).
    
* `CCSU`: The current usage of the CodeCache in kilobytes (K).
    
* `YGC`: The number of young generation garbage collections.
    
* `YGCT`: The total young generation garbage collection time in seconds.
    
* `FGC`: The number of full garbage collections.
    
* `FGCT`: The total full garbage collection time in seconds.
    
* `CGC`: The number of concurrent garbage collections.
    
* `CGCT`: The total concurrent garbage collection time in seconds.
    
* `GCT`: The total garbage collection time in seconds.
    

These values provide information about the memory usage and garbage collection activity of the Java application, which can be useful for tuning and performance analysis.

All of these are related to Garbage Collection.

Let's check ***<mark>CCSC</mark>*** in more detail.

The maximum value of `CCSC` (the current capacity of the CodeCache in kilobytes) depends on the configuration of the Java Virtual Machine (JVM) that is running your Java application.

By default, the CodeCache has a maximum size of 48 MB for the 32-bit version of the JVM and up to 192 MB for the 64-bit version. However, you can change the maximum size of the CodeCache by using the `-XX:ReservedCodeCacheSize` JVM option. The exact value of the `CCSC` column will depend on the size you specify for the `-XX:ReservedCodeCacheSize` option and the current usage of the CodeCache.

It's important to note that increasing the maximum size of the CodeCache will consume more memory, which may negatively impact the overall performance of the JVM and the host system. On the other hand, setting the maximum size too low may result in frequent CodeCache overflow and reduced performance of the JVM.