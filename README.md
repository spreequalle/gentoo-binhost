# armv5tel-softfloat-linux-musleabi

Packages optimized for Marvell Feroceon [88FR131](https://www.7-cpu.com/cpu/Kirkwood.html) (Sheeva 88SV131) cores.

<img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/88F6282A1C200.png" alt="88F6282" width="160" />

These cores can be found on the Marvell Kirkwood or Armada SoCs, for example:

* 88F6281
* 88F6282
* 88F6283

```
$ lscpu
Architecture:           armv5tel
  Byte Order:           Little Endian
CPU(s):                 1
  On-line CPU(s) list:  0
Vendor ID:              Marvell
  Model name:           Feroceon 88FR131
    Model:              1
    Thread(s) per core: 1
    Core(s) per socket: 1
    Socket(s):          1
    Stepping:           0x2
    CPU max MHz:        1800.0000
    CPU min MHz:        450.0000
    BogoMIPS:           400.00
    Flags:              swp half thumb fastmult edsp
```
## Usage

Binhost can be enabled by adding these lines to the **make.conf**.

```python
# enable binhost
PORTAGE_BINHOST="https://raw.githubusercontent.com/spreequalle/gentoo-binhost/${CHOST}"
FEATURES="${FEATURES} getbinpkg"
```

## Details

### Profile

Packages are generated using gentoo 17.0 [musl](https://www.musl-libc.org/) profile.

### USE Flags

```python
USE="brotli bzip2 lzma minimal lto mpfr png syslog expat gmp sqlite curl truetype icu zstd verify-sig"
USE="${USE} -debug -pam -nls -ipv6 -pax_kernel -threads -pic -hardened -openmp -filecaps -seccomp -xattr -tcpd -spell -openmp -berkdb -ldap"
```

### C Flags

Default optimizing using *-O2* with 32-bit [ARM](https://developer.arm.com/documentation/dui0473/m/overview-of-the-arm-architecture/arm--thumb--and-thumbee-instruction-sets) mode.

```python
CC="${CHOST}-gcc"
CXX="${CHOST}-g++"
AR=${CHOST}-gcc-ar
NM=${CHOST}-gcc-nm
RANLIB=${CHOST}-gcc-ranlib

CFLAGS_MAIN="-O2 -pipe -fno-ident -frename-registers -mfloat-abi=soft -fweb -fexcess-precision=fast -fomit-frame-pointer"

# 88fr131 cache configuration
CFLAGS_CPU_CACHE="--param l1-cache-size=16 --param l1-cache-line-size=32 --param l2-cache-size=256"
CFLAGS_CPU="-march=armv5te -mcpu=xscale ${CFLAGS_CPU_CACHE}"

CFLAGS_MODE_ARM="-marm"
CFLAGS_MODE_THUMB="-mthumb"
CFLAGS_MODE="${CFLAGS_MODE_ARM}"

CFLAGS_LTO="-flto -fuse-linker-plugin"

CFLAGS="${CFLAGS_MAIN} ${CFLAGS_CPU} ${CFLAGS_MODE} ${CFLAGS_LTO}"
CXXFLAGS="${CFLAGS} -fvisibility-inlines-hidden"
```

### LD Flags

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization) and [GOLD](https://en.wikipedia.org/wiki/Gold_(linker)) linker plugin.

```python
LDFLAGS_GENTOO="-Wl,-O1 -Wl,--as-needed"
LDFLAGS_MAIN="-Wl,--hash-style=gnu -Wl,--enable-new-dtags"

LDFLAGS_LTO="-flto -fuse-linker-plugin"

LDFLAGS_LD_BFD="-Wl,-fuse-ld=bfd"
LDFLAGS_LD_GOLD="-Wl,-fuse-ld=gold"

LDFLAGS_CPU="-march=armv5te -mcpu=xscale"
LDFLAGS="${LDFLAGS_GENTOO} ${LDFLAGS_MAIN} ${LDFLAGS_LTO} ${LDFLAGS_LD_GOLD} ${LDFLAGS_CPU}"
```
