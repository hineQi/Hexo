---
title: WebSocket、SockJs、STOMP三者关系
tags:
  - websocket
categories:
  - - 学习
    - websocket
date: 2020-02-09 15:13:00
---

## 1.http与websocket
http超文本传输协议，当一个网页打开完成后，客户端和服务器之间用于传输http的TCP连接不会关闭，再次访问会继续使用这一条已经建立的连接，但是只能是客户端主动请求才能获取返回数据，后台服务器不能主动推送给客户端数据

websocket是html5新增加的特性之一，目的是浏览器与服务端建立全双工的通信方式，http与websocket都是基于TCP的，websocket可以看作是对http的一个补充升级

## 2.SockJs
SockJs是一个JavaScript库，为了应对许多浏览器不支持WebSocket协议的问题，设计了备选SockJs。如果浏览器不支持WebSocket，则会自动降为轮询的方式。

## 3.STOMP
STOMP是面向消息的简单文本协议，SockJS为Websocket提供了备选方案，但是无论哪种场景对于实际应用，这种通信形式层级都是底层的，STOMP来为客户端和服务端的通信增加适当的消息语义。

## 4.WebSocket、SockJs、STOMP三者关系
简而言之，WebSocket 是底层协议，SockJS 是WebSocket 的备选方案，也是底层协议，而 STOMP 是基于 WebSocket（SockJS）的上层协议。

1、HTTP协议解决了 web 浏览器发起请求以及 web 服务器响应请求的细节，假设 HTTP 协议 并不存在，只能使用 TCP 套接字来 编写 web 应用。

2、直接使用 WebSocket（SockJS） 就很类似于 使用 TCP 套接字来编写 web 应用，因为没有高层协议，就需要我们定义应用间所发送消息的语义，还需要确保连接的两端都能遵循这些语义；

3、同HTTP在TCP 套接字上添加请求-响应模型层一样，STOMP在WebSocket 之上提供了一个基于帧的线路格式层，用来定义消息语义；