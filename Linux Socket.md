# Linux Socket Programming

## API Quick
```c
socket() // create socket
bind() // bind to a host:port
listen() // listen for connection
accept() // acception connection
write() // send
read() // recv
close() // close socket
inet_addr() // convert str ip to number addr
inet_ntoa() // convert addr to str ip
htons() // host to network order
ntohs() // network to host order
getsocketname() // get binded socket addr
getpeername() // get connected(remote) socket addr
perror(char* msg) // print error msg
strerror(int errno) // get errno corresponse error msg
```