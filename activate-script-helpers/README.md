## How to use

See the comments at the top of each activate script for instructions. The names are self-explanatory.

## What it does

Showing the output demonstrates it best

```
user at host in [~]   
16:53:45 › source /toolchains/musl/mipsel-sf-linux-musl/activate
--- Build environment setup ---

TOOLCHAIN_TARGET: mipsel-sf-linux-musl
TOOLCHAIN_BIN: /toolchains/musl/mipsel-sf-linux-musl/bin

  Symlinking gcc
  Symlinking addr2line
  Symlinking ar
  Symlinking as
  Symlinking c++
  Symlinking cpp
  Symlinking g++
  Symlinking ld
  Symlinking nm
  Symlinking objdump
  Symlinking ranlib
  Symlinking strip

TOOLCHAIN_TARGET: 	mipsel-sf-linux-musl
TOOLCHAIN_ROOT:	/toolchains/musl/mipsel-sf-linux-musl
TOOLCHAIN_BIN: 	/toolchains/musl/mipsel-sf-linux-musl/bin

GCC location: /toolchains/musl/mipsel-sf-linux-musl/bin/gcc
GAS location: /toolchains/musl/mipsel-sf-linux-musl/bin/as
GLD location: /toolchains/musl/mipsel-sf-linux-musl/bin/ld
G++ location: /toolchains/musl/mipsel-sf-linux-musl/bin/g++
CC location:  /toolchains/musl/mipsel-sf-linux-musl/bin/cc

UTIL_STATIC:      /crypt-big/toolchains/musl/mipsel-sf-linux-musl/mipsel-sf-linux-musl/lib/libutil.a
C_STATIC:         /crypt-big/toolchains/musl/mipsel-sf-linux-musl/mipsel-sf-linux-musl/lib/libc.a
DL_STATIC:        /crypt-big/toolchains/musl/mipsel-sf-linux-musl/mipsel-sf-linux-musl/lib/libdl.a
PTHREAD_STATIC:   /crypt-big/toolchains/musl/mipsel-sf-linux-musl/mipsel-sf-linux-musl/lib/libpthread.a
STDCXX_STATIC:    /crypt-big/toolchains/musl/mipsel-sf-linux-musl/mipsel-sf-linux-musl/lib/libstdc++.a
GCCEH_STATIC:     /crypt-big/toolchains/musl/mipsel-sf-linux-musl/lib/gcc/mipsel-sf-linux-musl/6.4.0/libgcc_eh.a

Use cross_configure to invoke alias for:
  $./configure --host=mipsel-sf-linux-musl --prefix=/toolchains/musl/mipsel-sf-linux-musl


user at host in [~]   
16:53:47 › 
```

This provides a few things:

* A simple way to one-off/simple builds (i.e. gcc somefile.c)
* A quick alias for using `./configure` based builds (via `cross_configure`)
* An easy way to to static linking

### Static Linking

The activate script makes static linking a little bit easier as it sets the following in the environment

`$UTIL_STATIC` - the path to the toolchain `libutil.a`
`$C_STATIC` - the path to the toolchain `libc.a`
`$DL_STATIC` - the path to the toolchain `libdl.a`
`$PTHREAD_STATIC` - the path to the toolchain `libpthread.a`
`$STDCXX_STATIC` - the path to the toolchain `libstdc++.a`
`$GCCEH_STATIC` - the path to the toolchain `libgcc_eh.a`

This way, when you see a build command like `gcc file1.c file2.c main.c -o program -ldl -lutil` you can easily replace it with `gcc file1.c file2. main.c -o program -l$DL_STATIC -l$UTIL_STATIC -static` and come out with a statically linked executable, no need to hunt down the individual libraries

## Tested with

* musl based toolchains produced by musl-cross-make
* openwrt based toolchains
* realtek sdk (rsdk) based toolchains

## Probably works with ...

Should work, possibly with small changes, on just about any third party toolchain, whether it uses uclibc or musl, possibly even glibc (untested)