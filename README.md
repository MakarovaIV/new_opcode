# new_opcode
## Description
This patch optimize opcodes `LOAD_FAST` and `LOAD_CONST` into `LOAD_OTUS`.

# Installation
- Install necessary programs/libs.

Command examples for CentOS 7
```commandline
yum install git -y
yum install -y gcc-c++
yum install -y make
```

- Clone Python 3.7 source code
```commandline
git clone -b 3.7 --single-branch https://github.com/python/cpython.git
```

- Copy new_opcode.patch to /cpython
- Make sure patch is ok

```commandline
git apply --check new_opcode.patch
```

- Apply patch
```commandline
git am --signoff < new_opcode.patch
```

- Compile Python
```commandline
./configure

make -j2
```

- Run Python
```commandline
./python
```

- Execute command
```commandline
def fib(n): return fib(n - 1) + fib(n - 2) if n > 1 else n
import dis
dis.dis(fib)
```

Make sure the result is similar to example
```commandline
>>> def fib(n): return fib(n - 1) + fib(n - 2) if n > 1 else n
... 
>>> import dis
>>> dis.dis(fib)
  1           0 LOAD_OTUS                1
              2 COMPARE_OP               4 (>)
              4 POP_JUMP_IF_FALSE       26
              6 LOAD_GLOBAL              0 (fib)
              8 LOAD_OTUS                1
             10 BINARY_SUBTRACT
             12 CALL_FUNCTION            1
             14 LOAD_GLOBAL              0 (fib)
             16 LOAD_OTUS                2
             18 BINARY_SUBTRACT
             20 CALL_FUNCTION            1
             22 BINARY_ADD
             24 RETURN_VALUE
        >>   26 LOAD_FAST                0 (n)
             28 RETURN_VALUE
```