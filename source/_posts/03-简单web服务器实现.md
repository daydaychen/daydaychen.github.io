---
title: 简单web服务器python实现
date: {{ date }}
tags: python
categories:
comments: true
---


```
import socket
import re


def service_client(new_socket):
    '''为这个客户端返回数据'''
    
    # 1. 接受浏览器发送过来的请求，即http请求
    # GET / HTTP/1.1
    # .....
    request = new_socket.recv(1024).decode('utf-8')
    
    request_lines = request.splitlines()
    print(request_lines)
    
    file_name = ''
    ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
    if ret:
        file_name = re.group(1)
        if file_name == "/":
            file_name = "/index.html"
    
    
    # 2. 返回http格式的数据，给浏览器
    
    try:
        f = open("./html/index.html","rb")
    except:
        response = 'HTTP/1.1 404 NOT FOUND\r\n'
        response += '\r\n'
        response += '----file not found----'
        new_socket.send(response.encode('utf-8'))
    else:
        # 2.1 准备发送给浏览器的数据 ---header---
        response = "HTTP/1.1 200 OK\r\n"
        response += "\r\n"
        # 2.2 准备发送给浏览器的数据 ---body---
        html_content = f.read()
        f.close()
        
        # 3. 发送数据
        # 3.1 发送header
        new_socket.send(response.encode("utf-8"))
        # 3.2 发送body
        new_socket.send(html_content)
    
    # 4. 关闭套接字
    new_socket.close()


def main():
    """用来完成整体的控制"""
    # 1. 创建套接字
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # 2. 绑定端口
    tcp_server_socket.bind(("", 7890))
    
    # 3. 变为监听套接字
    tcp_server_socket.listen(128)
    
    while True:
        # 4. 等待新客户端的链接
        new_socket, client_addr = tcp_server_socket.accept()
        
        # 5. 为这个客户端服务
        service_client(new_socket)
    
    # 关闭监听套接字
    tcp_server_socket.close()


if __name__ == "__main__":
    main()
```

#### TCP三次握手、四次挥手
