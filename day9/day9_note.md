##今日内容
+   socket编程之sockerserver

##简介:
    SocketServer内部使用 IO多路复用 以及 “多线程” 和 “多进程” ，从而实现并发处理多个客户端请求的Socket服务端。即：每个客户端请求连接到服务器时，
    Socket服务端都会在服务器是创建一个“线程”或者“进程” 专门负责处理当前客户端的所有请求。
    1、ThreadingTCPServer基础
        使用ThreadingTCPServer:
        创建一个继承自 SocketServer.BaseRequestHandler 的类
        类中必须定义一个名称为 handle 的方法
        启动ThreadingTCPServer
##例子
    SocketServer实现服务器
    
        #!/usr/bin/env python3
        # -*- coding:utf-8 -*-
        import SocketServer
        
        class MyServer(SocketServer.BaseRequestHandler):
        
            def handle(self):
                # print self.request,self.client_address,self.server
                conn = self.request
                conn.sendall('欢迎致电 10086，请输入1xxx,0转人工服务.')
                Flag = True
                while Flag:
                    data = conn.recv(1024)
                    if data == 'exit':
                        Flag = False
                    elif data == '0':
                        conn.sendall('通过可能会被录音.balabala一大推')
                    else:
                        conn.sendall('请重新输入.')
        
        if __name__ == '__main__':
            server = SocketServer.ThreadingTCPServer(('127.0.0.1',8009),MyServer)
            server.serve_forever()
    客户端     
      
        #!/usr/bin/env python
        # -*- coding:utf-8 -*-        
        import socket
        ip_port = ('127.0.0.1',8009)
        sk = socket.socket()
        sk.connect(ip_port)
        sk.settimeout(5)       
        while True:
            data = sk.recv(1024)
            print 'receive:',data
            inp = raw_input('please input:')
            sk.sendall(inp)
            if inp == 'exit':
                break
        
        sk.close()


        

