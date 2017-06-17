## Patch for mips32rel2 architecture

To address: https://github.com/mzpqnxow/gdb-static-cross/issues/1

Apply with:

```
user@host:~/gdb-7.12/$ patch -p1 < /path/to/gdb-7.12-mips32rel2-sigprocmask-bug.patch
```

Build as usual
