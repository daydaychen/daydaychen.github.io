---
title: 并发web服务器python实现
date: {{ date }}
tags: python
categories:
comments: true
---


#### 单进程、单线程、非阻塞实现监听多个套接字
```python
import socket
import time

tcp_server_tcp = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # 创建套接字
tcp_server_tcp.bind("",7890)    # 绑定端口
tcp_server_tcp.listen()     # 设为监听模式
tcp_server_tcp.setblocking(False)   # 设置套接字为非堵塞

client_socket_list = list()

while True:
    time.sleep(0.5)

    try:
        new_socket, new_addr = tcp_server_tcp.accept()
    except Exception as ret:
        print("---没有新的客户端到来---")
    else:
        print("---只要没有产生异常，那么也就意味着 来了一个新的客户端---")
        new_socket.setblocking(False)  # 设置套接字为非堵塞
        client_socket_list.append(new_socket)
        
    for client_socket in client_socket_list:
        try:
            client_socket.recv(1024)    
        except Exception as ret:
            print("---这个客户端没有发送过来数据---")
        else:
            if recv_data:
                # 对方发送过来数据
                print("---客户端发送过来了数据---")
            else:
                # 对方调用close recv_data返回值为空
                client_socket_list.remove(client_socket)
                client_socket.close()
                print("客户端已经关闭。")
```

#### 短连接 和 长连接
**长连接：** 一个套接字获取多次数据
**短连接：** 一个套接字只能获取一次数据

HTTP/1.0 是短连接
HTTP/1.1 是长连接

```
import socket
import re

def service_client(new_socket, request):
    '''为这个客户端返回数据'''
    # 1. 处理浏览器发送过来的数据
    request_lines = requests.splitlines()
    print("")
    print(">"*20)
    print(request_lines)
    
    file_name = ""
    ret = re.match(r"[^/]+(/[^ ]*)",request_lines[0])
    if ret:
        file_name = ret.group()
        if file_name == "/":
            file_name = "/index.html"
    
    # 2. 返回http格式的数据，给浏览器
    
    try:
        f = open("./html" + file_name, "rb")
    except:
        response = 'HTTP/1.1 404 NOT FOUND\r\n' 
        response += '\r\n' 
        response += '------file not found------'
        new_socket.send(response.encode('utf-8'))
    else:
        html_content = f.read()
        f.close()

        response_body = html_content
        
        response_header = 'HTTP/1.1 200 OK\r\n'
        response_header += 'Content-Length:%d\r\n' % len(response_body)
        response_header += '\r\n'

        response = response_header.encode('utf-8') + response_body
        
        new_socket.send(response)

def main():
    '''用来完成整体的控制'''
    # 1. 创建套接字
    tcp_server_tcp = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    
    # 2. 绑定
    tcp_server_socket.bind(("", 7890))
    
    # 3. 变为监听套接字
    tcp_server_socket.listen(128)
    tcp_server_socket.setblocking(False)  # 设为非阻塞
    
    # --------------
    # 创建一个epoll对象
    epl = select.epoll()
    
    # 将监听套接字对应的文件描述符fd注册到epoll中
    epl.register(tcp_server_socket.fileno(), select.EPOLLIN)
    # --------------
    client_socket_list = list()
    while True:
        # 4. 等待新客户端的链接
        try:
            new_socket, client_addr = tcp_server_socket.accept()
        except Exception as ret:
            pass
        else:
            new_socket.setblocking(False)
            client_socket_list.append(new_socket)
            
        for client_socket in client_socket_list:
            try:
                recv_data = client_socket.recv(1024).decode('utf-8')
            except Exception as ret:
                pass
            else:
                if recv_data:
                    service_client(client_socket, recv_data)
                else:
                    client_socket.close()
                    client_socket_list.remove(client_socket)
        
        # 关闭套接字
        tcp_server_socket.close()
```
#### epoll 
单进程单线程多任务 由应用程序采用轮询实现
epoll是事件通知类型，由内核实现
