# Java生产环境性能监控与调优详解学习笔记

# 1.课程目标收获

熟练使用各种监控和调试工具

应对生产环境中遇到的各种调试和性能问题

熟悉JVM的字节码指令

理解JVM的自动内存回收机制

GC调优

关于性能调优和调试的问题

## 1.1 基于JDK命令行工具的监控

JVM参数类型

查看运行时JVM参数

查看JVM统计信息

jmap+MAT实战内存溢出

jstack实战死循环与死锁

监控本地Java进程

监控远程Java进程

## 1.2 基于JVisualVM的可视化监控

## 1.3基于Btrace的监控调试

Btrace安装使用入门

Btrace使用详解

## 1.4 Tomcat性能监控与调优

Tomcat远程debug

Tomcat-manger监控Tomcat

psi-probe监控Tomcat

Tomcat调优

## 1.5 Nginx性能监控与调优

ngx_http_stub_status监控连接信息

ngxtop监控请求信息

nginx-rrd图形化监控

nginx调优

（这里只是大体讲解，详细的还需要专门找资料）

## 1.6 JVM层GC调优

JVM内存结构（基于JDK1.8）

垃圾回收算法

垃圾收集器

GC日志格式与可视化日志分析工具

Tomcat的GC调优实战

## 1.7 Java代码层优化

JVM字节码指令与javap

i++与++i，字符串+拼接原理

常用代码优化方法

# 2.基于JDK命令行工具的监控

主要内容：JVM参数类型，运行时JVM参数查看，jstat查看虚拟机统计信息，jmap+MAT实战内存溢出，jstack实战死循环与死锁。

## 2.1 JVM参数类型

标准参数，X参数，XX参数

### 2.1.1 标准参数

-help

-server -client

-version -showversion

-cp -classpath

### 2.1.2 X参数

非标准化参数

-Xint: 解释执行

-Xcomp: 第一次使用就编译成本地代码

-Xmixed: 混合模式，JVM自己来决定是否编译

```shell
C:\Users\dell>java -version
java version "1.8.0_241"
Java(TM) SE Runtime Environment (build 1.8.0_241-b07)
Java HotSpot(TM) 64-Bit Server VM (build 25.241-b07, mixed mode)
```

命令行使用 java -Xint -version 可调换



### 2.1.3 XX参数分类

**Boolean类型：**

格式：-XX:[+-]<name> 表示启用[+]或者禁用[-]name属性

比如：-XX:+UseConcMarkSweepGC

​			-XX:+UseG1GC



**非Boolean类型：**

格式：-XX:<name>=<value> 表示name属性的值是value

比如： -XX:MaxGCPauseMillis=500 （表示JDK最大停顿时间为500毫秒）

​               XX:GCTimeRatio=19



**-Xmx（JVM最大内存）**  ：

不是X参数，是XX参数。

-Xmx等价于-XX:MaxHeapSize



**-Xms（JVM最小内存）**

不是X参数，是XX参数。

-Xms等价于 -XX:InitialHeapSize