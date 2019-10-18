# armv5te-88fr131x-linux-musleabi

Packages optimized for Marvell Feroceon [88FR131](https://www.7-cpu.com/cpu/Kirkwood.html) (Sheeva 88SV131) cores.

<a href="#"><img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/88F6282A1C200.png" alt="88F6282" width="160" height="160" /></a>

These cores can be found on the Marvell Kirkwood or Armada SoCs for example:

* 88F6281
* 88F6282
* 88F6283

```
$ lscpu
Architecture:        armv5tel
Byte Order:          Little Endian
CPU(s):              1
On-line CPU(s) list: 0
Thread(s) per core:  1
Core(s) per socket:  1
Socket(s):           1
Vendor ID:           Marvell
Model:               1
Model name:          Feroceon 88FR131
Stepping:            0x2
CPU max MHz:         1000.0000
CPU min MHz:         400.0000
BogoMIPS:            160.00
Flags:               swp half thumb fastmult edsp
```
## Usage

Binhost can be enabled by adding these lines to the **make.conf**.

```python
# enable binhost
PORTAGE_BINHOST="https://raw.githubusercontent.com/spreequalle/gentoo-binhost/armv5te-88fr131x-linux-musleabi"
FEATURES="${FEATURES} getbinpkg"
```

## Details

### Profile

Packages are generated using gentoo 17.0 [musl](https://www.musl-libc.org/) profile.

### USE Flags

```python
USE="bzip2 minimal mpfr"
USE="${USE} -acl -pam -nls -ipv6 -pax_kernel -threads -pic -hardened -openmp -filecaps -seccomp -xattr"
```

### C FLAGS

Aggressively optimizing for size by using *-Os* and  [Thumb](http://infocenter.arm.com/help/topic/com.arm.doc.ddi0344c/Beiiegaf.html) mode.

```python
CFLAGS_MAIN="-Os -pipe -fno-ident -frename-registers -msoft-float -fweb -fexcess-precision=fast -fomit-frame-pointer"

# 88fr131 cache configuration
CFLAGS_CPU_CACHE="--param l1-cache-size=16 --param l1-cache-line-size=32 --param l2-cache-size=256"
CFLAGS_CPU="-mcpu=xscale ${CFLAGS_CPU_CACHE}"
CFLAGS_LTO="-flto -fuse-linker-plugin"

CFLAGS="${CFLAGS_MAIN} ${CFLAGS_CPU} ${CFLAGS_LTO}"
CXXFLAGS="${CFLAGS} -fvisibility-inlines-hidden"
```
### LD FLAGS

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization) and [GOLD](https://en.wikipedia.org/wiki/Gold_(linker)) linker plugin.

```python
LDFLAGS_MAIN="-Wl,--hash-style=gnu -Wl,--enable-new-dtags"
LDFLAGS_LTO="-flto -fuse-linker-plugin"

LDFLAGS_LD_BFD="-Wl,-fuse-ld=bfd"
LDFLAGS_LD_GOLD="-Wl,-fuse-ld=gold"

LDFLAGS="${LDFLAGS_MAIN} ${LDFLAGS_LTO} ${LDFLAGS_LD_GOLD}"
```
