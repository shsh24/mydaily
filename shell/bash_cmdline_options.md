Retrieve bash command line options


```
$ ./test.sh 11 22 33 44 55
5                   <- echo ${#}         : the count of total options
11 22 33 44 55      <- echo ${@}         : the all options
11                  <- echo ${1}         : the first option, notice ${0} is the command itself
22                  <- echo ${2}         : the second option
55                  <- echo ${@: -1}     : the last opton
44 55               <- echo ${@: -2}     : the second to the last option
22 33 44            <- echo ${@: 2:3}    : the 3 options start from the second
33 44 55            <- echo ${@: 3:$#}   : all the options start from the third
33 44               <- echo ${@: 3:$#-3} : the ($#-3) options start from the third
44 55               <- echo ${@: -2:2}   : the last 2 options
```

The general rule is `${@:<start>:<length>}`.
