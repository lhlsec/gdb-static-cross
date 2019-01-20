## Pre-built gdbserver executables for many platforms

*NOTE: these were taken for my larger statically compiled executable repository @ https://github.com/mzpqnxow/embedded-toolkit which includes many more static bins (gawk, lsof, tcpdump, etc)*

This is a collection of statically compiled gdbserver executables for Linux for many embedded platforms. They were tested thoroughly (in real life use)  and work very well- most if not all should have the most compatible options, i.e. software floating point emulation, etc. Some are actually customized for a specific CPU (i.e. the lexra build) so you might need to try a few on an exotic target because I didn't document things well enough while building. If you don't use the right one or you will get funny errors or segmentation faults. An error that contain s a paren almost always means that the endianness is incorect. Note some of these were built with musl while some were built with uClibc. Note that not all are 7.12. While nearly all of the 7.7 (and the 6.8) build(s) have been tested, not *all* of the 7.12 have been tested- at least one bug has popped up (on mips32rel2) and been addressed, if you notice bugs in any others *please* let me know and I can patch gdb or fix build issues

## Inventory

```
gdbserver-7.7.1-armel-eabi5-v1-sysv:                               ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked
gdbserver-7.7.1-armhf-eabi5-v1-sysv:                               ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked
gdbserver-7.7.1-armel-v1:                                          ELF 32-bit LSB executable, ARM, version 1, statically linked
gdbserver-7.12-i486-sysv:                                          ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked
gdbserver-7.12-mipsel-mips32rel2-v1:                               ELF 32-bit LSB executable, MIPS, MIPS32 rel2 version 1, statically linked
gdbserver-7.12-mips-mips32rel2-v1:                                 ELF 32-bit MSB executable, MIPS, MIPS32 rel2 version 1, statically linked
gdbserver-7.12-mipsel-mips32rel2-v1-sysv:                          ELF 32-bit LSB executable, MIPS, MIPS32 rel2 version 1 (SYSV), statically linked
gdbserver-7.12-mips-mips32rel2-v1-sysv:                            ELF 32-bit MSB executable, MIPS, MIPS32 rel2 version 1 (SYSV), statically linked
gdbserver-7.7.1-mips-mips32-v1:                                    ELF 32-bit MSB executable, MIPS, MIPS32 version 1, statically linked
gdbserver-7.7.1-mipsel-mips32-v1:                                  ELF 32-bit LSB executable, MIPS, MIPS32 version 1 (SYSV), statically linked
gdbserver-7.7.1-mips-mips32-v1-sysv:                               ELF 32-bit MSB executable, MIPS, MIPS32 version 1 (SYSV), statically linked
gdbserver-7.12-mips64-mips64rel2-v1-sysv:                          ELF 64-bit MSB executable, MIPS, MIPS64 rel2 version 1 (SYSV), statically linked
gdbserver-7.12-mipsel64-mips64rel2-v1-sysv:                        ELF 64-bit LSB executable, MIPS, MIPS64 rel2 version 1 (SYSV), statically linked
gdbserver-7.12-mips64-mips64-v1-sysv:                              ELF 64-bit MSB executable, MIPS, MIPS64 version 1 (SYSV), statically linked
gdbserver-7.12-mipsel64-mips64-v1-sysv:                            ELF 64-bit LSB executable, MIPS, MIPS64 version 1 (SYSV), statically linked
gdbserver-7.12-mips-mips64-v1-sysv:                                ELF 32-bit MSB executable, MIPS, MIPS64 version 1 (SYSV), statically linked
gdbserver-7.12-mips64-mips-iii-v1-sysv:                            ELF 64-bit MSB executable, MIPS, MIPS-III version 1 (SYSV), statically linked
gdbserver-7.12-mipsel64-mips-iii-v1-sysv:                          ELF 64-bit LSB executable, MIPS, MIPS-III version 1 (SYSV), statically linked
gdbserver-7.12-mips-mips-iii-v1-sysv:                              ELF 32-bit MSB executable, MIPS, MIPS-III version 1 (SYSV), statically linked
gdbserver-7.7.1-mipsel-ii-v1:                                      ELF 32-bit LSB executable, MIPS, MIPS-II version 1, statically linked
gdbserver-7.7.1-mips-mips-ii-v1:                                   ELF 32-bit MSB executable, MIPS, MIPS-II version 1, statically linked
gdbserver-7.12-mips-mips-i-v1:                                     ELF 32-bit MSB executable, MIPS, MIPS-I version 1, statically linked
gdbserver-7.12-mipsel-i-v1-sysv:                                   ELF 32-bit LSB executable, MIPS, MIPS-I version 1 (SYSV), statically linked
gdbserver-6.8-mips-i-rtl819x-lexra:                                ELF 32-bit MSB executable, MIPS, MIPS-I version 1 (SYSV), statically linked
gdbserver-7.12-mips-i-rtl819x-lexra:                               ELF 32-bit MSB executable, MIPS, MIPS-I version 1 (SYSV), statically linked
gdbserver-7.12-mips-i-v1-rtl819x-rsdk-1.3.6-4181-EB-2.6.30-0.9.30: ELF 32-bit MSB executable, MIPS, MIPS-I version 1 (SYSV), statically linked
gdbserver-7.12-mips-i-v1-rtl819x-rsdk-1.3.6-5281-EB-2.6.30-0.9.30: ELF 32-bit MSB executable, MIPS, MIPS-I version 1 (SYSV), statically linked
gdbserver-7.7.1-mips-mips-i-v1-sysv:                               ELF 32-bit MSB executable, MIPS, MIPS-I version 1 (SYSV), statically linked
gdbserver-7.12-mips64-mips-iv-v1-sysv:                             ELF 64-bit MSB executable, MIPS, MIPS-IV version 1 (SYSV), statically linked
gdbserver-7.12-mipsel64-mips-iv-v1-sysv:                           ELF 64-bit LSB executable, MIPS, MIPS-IV version 1 (SYSV), statically linked
gdbserver-7.12-mips64-n32-rel2v1-v1-sysv:                          ELF 32-bit MSB executable, MIPS, N32 MIPS64 rel2 version 1 (SYSV), statically linked
gdbserver-7.12-mipsel64-n32-rel2v1-v1-sysv:                        ELF 32-bit LSB executable, MIPS, N32 MIPS64 rel2 version 1 (SYSV), statically linked
gdbserver-7.12-mips64-n32-mips64-v1-sysv:                          ELF 32-bit MSB executable, MIPS, N32 MIPS64 version 1 (SYSV), statically linked
gdbserver-7.12-mipsel64-n32-mips64-v1-sysv:                        ELF 32-bit LSB executable, MIPS, N32 MIPS64 version 1 (SYSV), statically linked
gdbserver-7.12-mips64-n32-mips-iii-v1-sysv:                        ELF 32-bit MSB executable, MIPS, N32 MIPS-III version 1 (SYSV), statically linked
gdbserver-7.12-mipsel64-n32-mips-iii-v1-sysv:                      ELF 32-bit LSB executable, MIPS, N32 MIPS-III version 1 (SYSV), statically linked
gdbserver-7.12-mips64-n32-mips-iv-v1-sysv:                         ELF 32-bit MSB executable, MIPS, N32 MIPS-IV version 1 (SYSV), statically linked
gdbserver-7.12-mipsel64-n32-mips-iv-v1-sysv:                       ELF 32-bit LSB executable, MIPS, N32 MIPS-IV version 1 (SYSV), statically linked
gdbserver-7.12-ppc-sysv:                                           ELF 32-bit MSB executable, PowerPC or cisco 4500, version 1 (SYSV), statically linked
gdbserver-7.12-x86_64-sysv:                                        ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked
```

These are not stripped, go ahead and strip them if you want. And once more, there are now many more than just gdbserver executables in this repository, explore the directory tree. Note the special rtl819x-lexra build. This is a build specific to rtl819x SoC with Lexra CPU. Lexra CPUs are a MIPS-I with some standard MIPS instructions not implemented, specifically unaligned/half-word loads, stores, shifts. Silly. Thanks to https://github.com/KrabbyPatty/rtl819x-toolchain for making a toolchain available to build this specific binary. I was unable to get gdb 7.7.1 to build, so it is version 6.8.

## Build notes for 8.2

### MIPS32 example

The following is for MIPS32, as you can see via the `-mips32` flag given to CFLAGS and CXXFLAGS. For other platforms you may want to use other `CFLAGS` and `CXXFLAGS` to influence the ABI/CPU you need to be compatible with. Just make sure you leave `-fPIC` and `-static`. Just a few examples for MIPS:

* `-mips32r2`
* `-mips2`
* `-mips3`
* `-mips4`
* `-mips64`
* ...

#### Step by step

1. Use musl-cross-make to build your toolchain
2. Activate the toolchain using Extract the activate script provided in this repository
3. `tar -xvf gdb-8.2.tar.xv` && cd gdb/gdb/gdbserver
4. `cross_configure` to configure the `gdbserver` build
5. Use `V=1 make CXXFLAGS='-fPIC -static -mips32` CFLAGS='-fPIC -static -mips32` to perform the first build. This will produce a dynamically linked `gdbserver` because of the final link command, which uses `-Wl,--dynamic-list=./proc-service.list`
6. Copy the command that links `gdbserver` into a note and remove the `-Wl,--dynamic-list=./proc-service.list` from it. Also, replace `-ldl` with the path to `libdl.a` in your toolchain. 
7. Copy this new command into the terminal and it will relink `gdbserver`, statically
8. All done, use `file gdbserver` to confirm it is statically linked and for the correct ABI/CPU and then you are all set!
