---
title: "Understanding Latency vs Throughput - system design"
seoTitle: "Latency vs Throughput - system design principle"
seoDescription: "This article will help you understand the difference between latency and throughput so you can design a system that is scalable with high performance"
datePublished: Sat May 27 2023 12:02:13 GMT+0000 (Coordinated Universal Time)
cuid: cli5xzb7200050ajh830t2jru
slug: understanding-latency-vs-throughput-system-design
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/zBfBXHCaLmk/upload/c5790e1bcea0415f30b275d5697941be.jpeg
tags: caching, system-design, latency, load-balancing, throughput

---

Understanding the difference between latency and throughput in a system design will help you design better system that is both scalable and high in performance. It is also an important metric to measure the performance of a system.

In this article, you will understand the difference between this two and how it can affect your system positively and negatively.

But what exactly is **Latency**?

### **What is latency?**

Latency refers to how long or how much time it takes a system to handle a request, this is often measured in milliseconds or microseconds.

An increase in latency or high latency will mean that your system takes longer time to process a request.

Let use a web server as an example.

Assuming it takes your web server 0.05 milliseconds to process a single request and probably due to network congestion, inefficient algorithm or load on resources, it now takes your server 2millisec to process a request, this is said to be high latency.

High latency slows down your system performance.

### **What is throughput?**

Throughput refers to the number of requests a system can handle at the same time. It is usually measured in requests per second, transactions per second, or even bits per seconds.

There are different factor's that can affect throughput:

1. Limited CPU
    
2. Memory capacity etc
    

An increase in throughput should always be considered when designing a system that is scalable. It is also widely considered when designing systems such as network, database or storage systems

Here are 4 ways to increase throughput in your system and increase scalability.

1. Load Balancing
    
2. Caching mechanisms
    
3. Optimized algorithm or data structure
    
4. Increase hardware capacity etc
    

### **Load Balancing**

This is when you use reverse proxy to distribute incoming requests to different servers such that each server is able to process requests without exceeding it's capacity.

Let's say we have 1 million request coming in through the load Balancer and you have 4 web servers, this requests will not be sent to a single server but rather distributed using an algorithm equally to this 4 servers. This approach can help increase throughput in your system at the cost of latency.

### Caching mechanisms

Caching is also an important factor when increasing throughput, and this is by storing frequently used data in a cache. This is to avoid fetching data too frequently from the primary database beyond the server capacity.

Caching can significantly reduce the need for repetitive computations or data retrieval, improving overall system throughput.

### **Optimized algorithm or data structure**

You need to always look for opportunities to improve your algorithm and data structure. Using an optimized algorithm can affect your system greatly positively.

### Increase hardware capacity

If your system is limited by your hardware resources, you should consider upgrading or increasing it. This could include increasing CPU cores, memory capacity, disk I/O speed, or network bandwidth to handle higher workloads.

In summary, There is great benefit when you consider latency and throughput when designing a system but there is a tradeoff, increasing your throughput often results in high latency so you should consider which to sacrifice and best for your system.