---
title: "Understanding the Output of JStat Command in Java"
seoTitle: "Explained: JStat Command Output and What Each Column Means"
seoDescription: "JStat is a powerful command-line tool for monitoring the performance of a Java application. But what do all those columns in the JStat output actually mean?"
datePublished: Sun Feb 19 2023 08:14:07 GMT+0000 (Coordinated Universal Time)
cuid: cleb44bsi000508mr5zuw6f9h
slug: understanding-the-output-of-jstat-command-in-java
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/npxXWgQ33ZQ/upload/fb18b6ff90a4b2bfd8d6da22eaff2fd9.jpeg
tags: java, command-line

---

```yaml
jstat -gc 12460
 S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT
 0.0   4096.0  0.0   2591.0 75776.0  69632.0   83968.0    48156.0   60416.0 59829.4 8832.0 8565.2     10    0.036   0      0.000    0.036
```

Java applications often run in complex environments, and monitoring their performance is crucial for ensuring optimal functionality. JStat, a command-line utility bundled with the Java Development Kit (JDK), provides valuable insights into the performance of Java applications by displaying various metrics related to garbage collection, memory usage, and more.

To run JStat, open a command prompt or terminal and type the following command:

```bash
jstat [options] <vmid> [interval] [count]
```

* `[options]`: Specifies the desired output format and the specific statistics to be displayed.
    
* `<vmid>`: The virtual machine ID, which can be obtained using the `jps` (Java Process Status) command.
    
* `[interval]`: The update interval for collecting statistics (in seconds).
    
* `[count]`: The number of times to display the statistics.
    

**Memory Usage (Option:** `-gc`**):**

The `-gc` option provides information about garbage collection. The output includes metrics like S0C (survivor space 0 capacity), S1C (survivor space 1 capacity), S0U (survivor space 0 utilization), S1U (survivor space 1 utilization), EC (eden space capacity), EU (eden space utilization), OC (old space capacity), OU (old space utilization), MC (metaspace capacity), and MU (metaspace utilization).

Example:

```bash
jstat -gc <vmid> 1s 10
```

**Class Loading (Option:** `-class`**):**

The `-class` option displays information about class loading and unloading. Key metrics include Loaded (loaded classes), Bytes (loaded bytes), Unloaded (unloaded classes), and UBytes (unloaded bytes).

Example:

```bash
jstat -class <vmid> 1s 10
```

**Thread Activity (Option:** `-gcutil`**):**

The `-gcutil` option provides information on JVM garbage collection statistics. Key metrics include S0 (survivor space 0 utilization), S1 (survivor space 1 utilization), E (eden space utilization), O (old space utilization), and YGC (young generation garbage collection count).

Example:

```bash
jstat -gcutil <vmid> 1s 10
```

Let's try to run **jstat** on a Java application, and we will see some values printed like this.

```yaml
jstat -gc 12460
 S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT
 0.0   4096.0  0.0   2591.0 75776.0  69632.0   83968.0    48156.0   60416.0 59829.4 8832.0 8565.2     10    0.036   0      0.000    0.036
```

Let's try to learn, what is the meaning of each value.

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

All of these are related to Garbage Collection, as we ran jstat with -gc option.

Let's check ***<mark>CCSC</mark>*** in more detail.

The maximum value of `CCSC` (the current capacity of the CodeCache in kilobytes) depends on the configuration of the Java Virtual Machine (JVM) that is running your Java application.

By default, the CodeCache has a maximum size of 48 MB for the 32-bit version of the JVM and up to 192 MB for the 64-bit version. However, you can change the maximum size of the CodeCache by using the `-XX:ReservedCodeCacheSize` JVM option. The exact value of the `CCSC` column will depend on the size you specify for the `-XX:ReservedCodeCacheSize` option and the current usage of the CodeCache.

It's important to note that increasing the maximum size of the CodeCache will consume more memory, which may negatively impact the overall performance of the JVM and the host system. On the other hand, setting the maximum size too low may result in frequent CodeCache overflow and reduced performance of the JVM.

JStat is a powerful tool for monitoring and understanding the performance of Java applications. By using its various options and interpreting the output, developers and administrators can gain insights into memory usage, garbage collection behavior, class loading, and thread activity.