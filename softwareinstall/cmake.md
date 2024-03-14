# Build cmake3 on HP-UX


Following the READM file.

Just one notice: 
- The '-Aa' C++ flag should be set, otherwise there are duplicated for-loop-scope-variable re-defined compiler error.
- Currently cmake version 3.2 ~ 3.9 are supported on HP-UX.

```
$ gunzip cmake-3.x.x.tar.gz | tar -xvf -
$ cd cmake-3.x.x

$ export CXXFLAGS="-Aa"
$ ./bootstrap --prefix=<prefix>
$ make
$ make install
```
