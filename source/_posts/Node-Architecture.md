---
title: Node Architecture
date: 2023-09-30 9:12:30
tags: Node.js
---

### 1. Core dependencies: 

1. Google V8 engine: convert JS code into machine code
2. libuv:
   1. gives node access to underlying computer operating system, file system, networking, etc.
   2. gives node two features: Event loop & thread pool

Btw, V8 and libuv are written by C++, so which allows us to write pure JS code running in Node.js, but still access funtions like file reading, which actually implemented in libuv or other libraries in C++

### 2. Other dependencies:

http-parser, c-ares, OpenSSL, zlib

![image-20230930091210811.png](https://github.com/ChengZirao/my-blog-deployment/blob/master/blog_img/image-20230930091210811.png?raw=true)