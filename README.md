## Build scripts and toolchain helpers and how to build a cross-compile and statically link gdbserver

First things first, I apologize for the poor documentation but I did not intend to write any. Hopefully it's better than nothing

### What this repository is NOT

This repository is not a toolchain or a toolchain builder

### What this repository *kind of* is

A HOWTO on building a statically linked gdbserver without much hacking

### What this repository IS when fully utilized

This repository is for when you don't want something as heavy as crosstool-ng or buildroot. You just need to cross-compile and statically link a few binaries, for example gdbserver, and you have musl/uClibc/glibc toolchain to work from already built. This focuses on gdb-7.12, but you can use these scripts in one form or another when building just about anything, though you will need to modify them from time to time to suit your needs. While they function out of the box for my needs, it is best you think of them as living documentation / reference material for a simpleish task that can be confusing for first-timers, and slow for veterans

* [activate-openwrt-toolchain.env](https://github.com/mzpqnxow/gdb-static-cross/blob/master/activate-script-helpers/activate-openwrt-toolchain.env) - place file in prebuilt OpenWRT toolchain root, source it for productivity, etc.
* [activate-musl-toolchain.env](https://github.com/mzpqnxow/gdb-static-cross/blob/master/activate-script-helpers/activate-musl-toolchain.env) - place file in musl-cross-make toolchain root, source it for productivity, etc.
* [gdbserver-7.12-static-build.sh](https://github.com/mzpqnxow/gdb-static-cross/blob/master/gdbserver-7.12-static-build.sh) - shell script to build a static gdb-7.12 gdbserver using a cross-compile toolchain. Should be executed from gdb-7.12-/gdb/gdbserver/ which is extracted from the [gdbserver-7.12.tar.gz](https://ftp.gnu.org/gnu/gdb/gdb-7.12.1.tar.gz) tarball from GNU distribution sites or from this repository
* [patches](https://github.com/mzpqnxow/gdb-static-cross/blob/master/patches/) - patches for various architectures and toolchains, required to build gdb-7.12

*Note that you can use the `--disable-build-with-cxx` option when configuring gdb-7.12/gdb/gdbserver in some cases to make your life easier*

### ATTENTION!

***If you just want to download a statically linked gdbserver for a specific MIPS(EL) or ARM platform, check the [prebuilt-static](https://github.com/mzpqnxow/gdb-static-cross/tree/master/prebuilt-static/) directory***

I tried to compile builds as portable as possible (i.e. emulating FP in software) and got a lot out of use of what was produced in real life situations, so you should find them somewhat reliable on Linux based embedded devices

### If you want to build things other than gdbserver

Some open source projects have no real build system, and many have very strange custom build systems that don't use the "standard" `./configure && make && make install` style build system . You can check out [embedded-toolkt](https://github.com/mzpqnxow/embedded-toolkit) and look in the [src](https://github.com/mzpqnxow/embedded-toolkit/tree/master/src) directory for examples of using these scripts before building various tools such as *gawk*, *gdbserver*, *tcpdump*, *libpcap*, *lsof*, *etc* which have some unique build systems, especially lsof... the lsof configure/build system is especially odd. Also, if you have a toolchain without wchar, use mawk instead of gawk. The mawk source and patches to build mawk are also included in [src](https://github.com/mzpqnxow/embedded-toolkit/tree/master/src/mawk). You can also use this for simple `gcc bah.c -o bah` operations or `gcc bah.s -o bah`

## OpenWrt: Use `source activate-openwrt-toolchain.env` with a pre-built OpenWrt Toolchain

To use `activate-openwrt-toolchain.env` with a pre-built **OpenWrt Toolchain** you can follow these simple steps. First browse to [https://downloads.openwrt.org/snapshots/trunk/](https://downloads.openwrt.org/snapshots/trunk/) to find the toolchain you want to use. You can use `gcc -v` to determine if the options are right for your CPU

To use this script, assume you have a directory called /toolchains/ and that this is where you will keep the toolchains, one subdirectory per toolchain. To get a new toolchain up in such a way to use active-openwrt-toolchain, grab a file like [OpenWrt-Toolchain-brcm63xx-generic_gcc-5.3.0_musl-1.1.16.Linux-x86_64.tar.bz2](https://downloads.openwrt.org/snapshots/trunk/brcm63xx/generic/OpenWrt-Toolchain-brcm63xx-generic_gcc-5.3.0_musl-1.1.16.Linux-x86_64.tar.bz2)

### For dummies: Build using OpenWRT and these scripts, step by step for the impatient cut and paste enthusiasts
```
$ sudo mkdir -p /openwrt-toolchains
$ sudo chown $USER /openwrt-toolchains
$ cd ~/ && git clone https://github.com/mzpqnxow/gdb-static-cross.git
$ cd gdb-static-cross.git
$ wget https://.../OpenWrt-Toolchain-brcm63xx-generic_gcc-5.3.0_musl-1.1.16.Linux-x86_64.tar.bz2
$ tar -xvjf OpenWrt-Toolchain-brcm63xx-generic_gcc-5.3.0_musl-1.1.16.Linux-x86_64.tar.bz2
$ cd OpenWrt-Toolchain-brcm63xx-generic_gcc-5.3.0_musl-1.1.16.Linux-x86_64/
$ TOOLCHAIN=$(echo toolchain-*)
$ mv "$TOOLCHAIN" /openwrt-toolchains/
$ cp ~/gdb-static-cross.git/activate-openwrt-toolchain.env /openwrt-toolchains/$TOOLCHAIN/activate
$ source /openwrt-toolchains/$TOOLCHAIN/activate
```

## Notes for GDB >= 8

There are different patches/procedures required with GDB >= 8. For instance, there is no flag to disable the use of c++ at build time.

There still does not appear to be a clean/clear way to build statically. So build as normal. Once things are built, do the following:


```
$ rm -f gdbserver gdbreplay
$ V=1 make
```

You should see the following at the end:

```
mips-sf-linux-musl-g++  -g -O2    -I. -I. -I./../common -I./../regformats -I./.. -I./../../include -I./../gnulib/import -Ibuild-gnulib-gdbserver/import  -Wall -Wpointer-arith -Wno-unused -Wunused-value -Wunused-function -Wno-switch -Wno-char-subscripts -Wempty-body -Wunused-but-set-parameter -Wunused-but-set-variable -Wno-sign-compare -Wno-narrowing -Wno-error=maybe-uninitialized -Wsuggest-override -Wduplicated-cond  -DGDBSERVER   \
	-o gdbserver ax.o common/agent.o common/btrace-common.o common/buffer.o common/cleanups.o common/common-debug.o common/common-exceptions.o common/job-control.o common/common-regcache.o common/common-utils.o common/errors.o common/environ.o common/fileio.o common/filestuff.o common/format.o common/gdb_tilde_expand.o common/gdb_vecs.o common/new-op.o common/pathstuff.o common/print-utils.o common/ptid.o common/rsp-low.o common/signals.o common/signals-state-save-restore.o common/tdesc.o common/vec.o common/xml-utils.o debug.o dll.o event-loop.o hostio.o inferiors.o mem-break.o notif.o regcache.o remote-utils.o server.o symbol.o target.o tdesc.o tracepoint.o utils.o version.o waitstatus.o mips-linux.o mips-dsp-linux.o mips64-linux.o mips64-dsp-linux.o linux-low.o linux-osdata.o linux-procfs.o linux-ptrace.o linux-waitpid.o linux-personality.o linux-namespaces.o fork-child.o fork-inferior.o linux-mips-low.o mips-linux-watch.o hostio-errno.o thread-db.o proc-service.o common/posix-strerror.o   xml-builtin.o build-gnulib-gdbserver/import/libgnu.a build-libiberty-gdbserver/libiberty.a \
	-ldl 
```

What you need to do here is pretty simple. Remove the `-Wl,--dynamic-list=./proc-service.list` from the final gdbserver build command and add the `-static` option. Replace `-ldl` with `$DL_STATIC` (assuming you used one of the activate scripts) *or*, replace with the path to your toolchains `libdl.a` file and then run the command over again. This ultimately looks like this:

```
mips-sf-linux-musl-g++  -g -O2    -I. -I. -I./../common -I./../regformats -I./.. -I./../../include -I./../gnulib/import -Ibuild-gnulib-gdbserver/import  -Wall -Wpointer-arith -Wno-unused -Wunused-value -Wunused-function -Wno-switch -Wno-char-subscripts -Wempty-body -Wunused-but-set-parameter -Wunused-but-set-variable -Wno-sign-compare -Wno-narrowing -Wno-error=maybe-uninitialized -Wsuggest-override -Wduplicated-cond  -DGDBSERVER -o gdbserver ax.o common/agent.o common/btrace-common.o common/buffer.o common/cleanups.o common/common-debug.o common/common-exceptions.o common/job-control.o common/common-regcache.o common/common-utils.o common/errors.o common/environ.o common/fileio.o common/filestuff.o common/format.o common/gdb_tilde_expand.o common/gdb_vecs.o common/new-op.o common/pathstuff.o common/print-utils.o common/ptid.o common/rsp-low.o common/signals.o common/signals-state-save-restore.o common/tdesc.o common/vec.o common/xml-utils.o debug.o dll.o event-loop.o hostio.o inferiors.o mem-break.o notif.o regcache.o remote-utils.o server.o symbol.o target.o tdesc.o tracepoint.o utils.o version.o waitstatus.o mips-linux.o mips-dsp-linux.o mips64-linux.o mips64-dsp-linux.o linux-low.o linux-osdata.o linux-procfs.o linux-ptrace.o linux-waitpid.o linux-personality.o linux-namespaces.o fork-child.o fork-inferior.o linux-mips-low.o mips-linux-watch.o hostio-errno.o thread-db.o proc-service.o common/posix-strerror.o   xml-builtin.o build-gnulib-gdbserver/import/libgnu.a build-libiberty-gdbserver/libiberty.a $DL_STATIC -static
```

Now you should have a statically linked gdbserver:

```$ file gdbserver 
gdbserver: ELF 32-bit MSB executable, MIPS, MIPS-I version 1 (SYSV), statically linked, not stripped
```

I have only had time/necessity to build 8.2 for MIPS/MSB so far but I will update the repository when I find all of my toolchains (hrmmm they are on one of these disks... d'oh!)


## For dummies: MUSL via [musl-cross-make](https://github.com/richfelker/musl-cross-make/): Use `source activate-musl-toolchain.env` with an installed toolchain built by musl-cross-make

Using [musl-cross-make](https://github.com/richfelker/musl-cross-make/) is a great experience and kudos to [@richfelker](https://github.com/richfelker) for making it available. I recommend you try it, even just for testing a sample i486 toolchain, useful for building statically linked libraries for common desktop/server platforms. If you do try it, all you need to do is edit the musl-cross-make config.mak file to specify the architecture and any other flags as well as installation path and then use make -j and make install. That's it. You're done. The activate-musl-toolchain file provided here is for you to place in the root of the installed toolchain to use as a convenience to "activate" the toolchain in your environment for use with *weird* build systems, or for software with no build system at all. You use it by simply `source`ing it in your active shell. Sorry, there is no deactivate. Just start a new shell to restore your environment. Here's an example

```
$ export TOOLCHAIN_DEST=/musl-cross-make-toolchains/toolchain-mips_mips32_musl/
$ cd ~/
$ git clone https://github.com/richfelker/musl-cross-make
$ cd musl-cross-make
$ vi config.mak
... assume you're installing to $TOOLCHAIN_DEST ...
$ make -j && make install
$ cd ~/
$ git clone https://github.com/mzpqnxow/gdb-static-cross.git
$ cd gdb-static-cross.git
$ cp activate-musl-toolchain.env $TOOLCHAIN_DEST/activate
$ source $TOOLCHAIN_DEST/activate
$ wget https://ftp.gnu.org/gnu/gdb/gdb-7.12.tar.xz
$ tar -xvf gdb-7.12.tar.xz
$ cp gdbserver-7.12-static-build.sh gdb-7.12/gdb/gdbserver
$ cd gdb-7.12/gdb/gdbserver
$ ./gdbserver-7.12-static-build.sh
$ file gdbserver
gdbserver: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked, not stripped
```

### Capabilities provided by the shell source scripts

**WARN source these files from a bash shell only, not zsh, csh, etc..**

Look at the end of each .env file. You will see some variables exported, hopefully intuitively named. Those variables can now be accessed in your shell while building software with your toolchain with no need to adjust paths or locate static libraries. Tools like gcc, ar, as, ld, g++, etc. will also now be in your path and there will be a `cross_configure` alias in the shell to simplify using software packages that utilize `./configure` build systems, just replace `./configure` with `cross_configure`. This is a very simple alias that just includes the `--host` and `--prefix` options set, it's not magic.

#### Sample environment variables set after activating an OpenWrt toolchain

```
DL_STATIC=/opt/openwrt/armel-gnu-eabi5-sysv/lib/libdl.a
C_STATIC=/opt/openwrt/armel-gnu-eabi5-sysv/lib/libc.a
STDCXX_STATIC=/opt/openwrt/armel-gnu-eabi5-sysv/lib/libstdc++.a
UTIL_STATIC=/opt/openwrt/armel-gnu-eabi5-sysv/lib/libutil.a
PTHREAD_STATIC=/opt/openwrt/armel-gnu-eabi5-sysv/lib/libpthread.a
GCCEH_STATIC=/opt/openwrt/armel-gnu-eabi5-sysv/lib/gcc/arm-openwrt-linux-muslgnueabi/5.3.0/libgcc_eh.a
STAGING_DIR=/opt/openwrt/armel-gnu-eabi5-sysv
TOOLCHAIN_ROOT=/opt/openwrt/armel-gnu-eabi5-sysv
TOOLCHAIN_BIN=/opt/openwrt/armel-gnu-eabi5-sysv/bin
TOOLCHAIN_TARGET=arm-openwrt-linux-muslgnueabi
SYSTEM_ROOT=/opt/openwrt/armel-gnu-eabi5-sysv
```

#### Sample alias that you will find in your environment

`alias cross_configure='./configure --host=arm-openwrt-linux-muslgnueabi --prefix=/opt/openwrt/armel-gnu-eabi5-sysv'`

### Doing a plain old build of gdbserver for the same host and target (i.e. no special toolchain, 'native' build)

*This is not something you will be doing often as  with gdbserver you will almost always be using a non-native toolchain. However, I wanted to include an example of how it can be cleanly done in a standard build environment. It assumes glibc, which is not really a good target for building non-trivial statically executables. With a minimal amount of effort you can build a musl toolchain for x86_64 or i486*

#### Find static libraries you will need to link gdbserver with

You will need to make sure you have libstdc++.a and libgcc_eh.a on your system. If you don't know what you're doing, you can just use find. Try using the following:

```
$ find /l* /u* -name libgcc_eh.a
... choose the appropriate one if you have a multilib system ..
$ export LIBGCC=/usr/lib/gcc/x86_64-linux-gnu/4.9/libgcc_eh.a
$ find /l* /u* -name libstdc++.a
... choose the appropriate one if you have a multilib system ..
$ export LIBCXX=/usr/lib/gcc/x86_64-linux-gnu/4.9/libstdc++.a
$ export CC=gcc-4.9
... set this if you have multiple versions of gcc on your system ...
```

#### Perform the build

```
$ wget https://ftp.gnu.org/gnu/gdb/gdb-7.12.tar.xz
$ tar -xvf gdb-7.12.tar.xz
$ cd gdb-7.12/gdb/gdbserver
$ sed -i -e 's/srv_linux_thread_db=yes//' configure.srv
... optionally apply architecture specific patches found in patches/ as required by your toolchain or architecture ...
$ ./configure --prefix=/opt/gdbserver-7.12-static CXXFLAGS='-fPIC -static'
$ make -j gdbserver GDBSERVER_LIBS="$LIBGCC $LIBCXX"
```

You should now have a statically compiled GDB 7.12 gdbserver for your native OS. Read on for the cross-compile stuff, which is a little more involved but still pretty simple.

*NOTE: You really should use uClibc or musl for your libc, not glibc, because of libnss which requires dynamic library loading via libdl*

## Notes on stubborn build targets

When you encounter a target that still builds a dynamic executable, even using `cross_configure` alias, try the following:

1. Let the build go until it produces the dynamically linked executable
2. Delete the executable(s) using `rm` but leave the object (.o) files
3. Run `V=1 make` (without `-j`) to cause the final linkining command(s) to print to stdout
4. Paste the final linking command(s) into a file for easy editing
5. Everywhere you see a -l<lib> replace with a /path/to/<lib>.a
6. (Optional) In some cases (gdb 8.x) you will also need to replace 
7. Paste the (now modified) link command into your shell and it should link properly

## GDB packet errors

If you run into something like this:

```
(gdb) target remote 10.1.2.132:12346
Remote debugging using 10.1.2.132:12346
Malformed packet(b) (missing colon): ore:0;
Packet: 'T001d:7fa08ea8;25:77668244;thread:2db0;core:0;'

```

Then you should invoke gdbserver with the following (or similar)

```
$ gdbserver :<port> --disable-packet=threads <pid>
```

## License

* The shell scripts and source files here are is released under the terms of GPLv2 by copyright@mzpqnxow.com
* Please see LICENSE or LICENSE.md for more information on GPLv2
* Third party software is included with license information intact
