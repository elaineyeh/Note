# How to use socket open html in 80 port
Use socket instead of SimpleHTTPServer
### Only root can use 1~1023 port
```python
import socket

serversocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM) #creates a socket object
serversocket.bind(("localhost", 80)) #setting ip and port
serversocket.listen(10) #It specifies the number of unaccepted connections that the system will allow before refusing new connections.

msg = b"""HTTP/1.1\r\nContent-Type:text/html\r\n\r\n<html>\r\n<body>\r\n<H1>Hello World</H1>\r\n</body>\r\n</html>\r\n"""

while True:
    (clientsocket, address) = serversocket.accept() #accept client
    receive = clientsocket.recv(1024) #receive the message which client sends
    sent = clientsocket.send(msg) #server send message
    clientsocket.close() #close the connect

```
```shell=
sudo python echo_server.py
```

### Reference:
[HTTP_message_body](https://en.wikipedia.org/wiki/HTTP_message_body)
[python-sockets](https://realpython.com/python-sockets/)
