
1. Start a SSL server:
```
$ openssl s_server -accept <port> -key path/to/server.key -cert path/to/server.pem
```

2. Client connect to server
```
$ openssl s_client -connect <hostname>:<port> -CAfile path/to/ca.pem
```


And there are other options you can specify, for example:
```
* -cipher AES128-SHA
* -tls1_2
```
