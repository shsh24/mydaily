# A gcc compile error when upgrade grom gcc-4.8 to gcc-8.5

The compile error can be reproduced using this code snippet:
```
#include <cmath>

int main(int argc, char *argv[])
{
    return isnan(1.0);
}
```

```
$ g++ --version
g++ (GCC) 8.5.0 20210514 (Red Hat 8.5.0-18.0.6)
...
$ g++ test.cpp
test.cpp: In function ‘int main(int, char**)’:
test.cpp:5:12: error: ‘isnan’ was not declared in this scope
     return isnan(1.0);
            ^~~~~
test.cpp:5:12: note: suggested alternative:
In file included from test.cpp:1:
/usr/include/c++/8/cmath:632:5: note:   ‘std::isnan’
     isnan(_Tp __x)
     ^~~~~
```

* Solution 1: update source code to add 'std' namespace prior to function isnan()
```
#include <cmath>

int main(int argc, char *argv[])
{
    return std::isnan(1.0);
}
```

* Solution 2: use '--std:c++98' command line option
```
$ g++ --std=c++98 test.cpp
```

For details of C++ Standards Support in GCC, please refer to https://gcc.gnu.org/projects/cxx-status.html

And for convenient compile process, the g++ command can be overwrite with alias in ~/.bashrc file:
```
$ echo "alias g++='g++ -std=c++98'" >> ~/.bashrc
```
