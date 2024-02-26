# Specify a given TLS version for curl command line

Code Sample:
```
$ curl -v --tlsv1.2 --tls-max 1.2 --cacert path/to/ca.pem https://<server>:<port>
```

Options:
1. --tlsv1.0/--tlsv1.1/--tlsv1.2/--tlsv1.3

   Set the minimun TLS version
3. --tls-max <VER>

   Set the maximum TLS version
