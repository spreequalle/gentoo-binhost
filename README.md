# powerpc-74xx-linux-gnu

Packages optimized for the 32-bit PowerPC [74xx](https://en.wikipedia.org/wiki/PowerPC_G4) line of processors.

<img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/MC7447A.png" alt="MC7447A" width="160" />

These CPU cores there produced by different manufacturers (Motorola, IBM, Freescale/NXP) and there most prominently used in Apple G4 product line.

```
$ lscpu
Architecture:        ppc
CPU op-mode(s):      32-bit
Byte Order:          Big Endian
CPU(s):              1
On-line CPU(s) list: 0
Thread(s) per core:  1
Core(s) per socket:  1
Socket(s):           1
Model:               1.1 (pvr 8003 0101)
Model name:          7447A, altivec supported
CPU max MHz:         1499,9990
CPU min MHz:         749,9990
BogoMIPS:            73.72
L1d cache:           32K
L1i cache:           32K
L2 cache:            512K
```

## Usage

Binhost can be enabled by adding these lines to the **make.conf**.

```bash
# enable binhost
PORTAGE_BINHOST="https://raw.githubusercontent.com/spreequalle/gentoo-binhost/${CHOST}"
FEATURES="${FEATURES} getbinpkg"
```

## Details

### Profile

Packages are generated using gentoo 17.0 systemd [glibc](https://www.gnu.org/software/libc/) profile.

### USE Flags

```bash
USE="-nls -tcpd"
USE="${USE} altivec lzma lzo curl gmp elf"
```

### C FLAGS

```bash
CFLAGS_MAIN="-O2 -pipe -fno-strict-aliasing -frename-registers -msecure-plt -fivopts -fsigned-char -fomit-frame-pointer -fno-ident -fexcess-precision=fast"
CFLAGS_CPU_CACHE="--param l1-cache-size=32 --param l1-cache-line-size=32 --param l2-cache-size=512"
CFLAGS_CPU="-mcpu=7450 -mtune=7450 -maltivec -mabi=altivec ${CFLAGS_CPU_CACHE}"
CFLAGS_LTO="-flto -fuse-linker-plugin"
CFLAGS="${CFLAGS_MAIN} ${CFLAGS_CPU} ${CFLAGS_LTO}"
CXXFLAGS="${CFLAGS} -fvisibility-inlines-hidden"
```

### LD FLAGS

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization).

```bash
LDFLAGS_MAIN="-Wl,--hash-style=gnu -Wl,--enable-new-dtags"
LDFLAGS_LDBFD="-Wl,-fuse-ld=bfd"
LDFLAGS_LDGOLD="-Wl,-fuse-ld=gold"
LDFLAGS_LTO="-flto -fuse-linker-plugin"
LDFLAGS="${LDFLAGS_MAIN} ${LDFLAGS_LDGOLD} ${LDFLAGS_LTO}"
```
