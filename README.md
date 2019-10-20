# armv7a-88sv584x-linux-gnueabihf

Packages optimized for Marvell Sheeva PJ4B-MP cores.

<img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/MV78460.png" alt="88F6282" width="160" />

These cores can be found on the Marvell Kirkwood or Armada XP SoCs for example:

* MV78230
* MV78260
* MV78460

```
$ lscpu
Architecture:        armv7l
Byte Order:          Little Endian
CPU(s):              4
On-line CPU(s) list: 0-3
Thread(s) per core:  1
Core(s) per socket:  4
Socket(s):           1
Vendor ID:           Marvell
Model:               2
Model name:          PJ4B-MP
Stepping:            0x2
CPU max MHz:         1333.0000
CPU min MHz:         666.5000
BogoMIPS:            50.00
Flags:               half thumb fastmult vfp edsp thumbee vfpv3 tls idiva idivt vfpd32 lpae
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

Packages are generated using gentoo 17.0 [glibc](https://www.gnu.org/software/libc/) profile.

### USE Flags

```python
USE="-acl -cups -nls -openmp -tcpd"
USE="${USE} bindist curl elf gmp gold lz4 lzma lzo zstd systemd"
```

### C FLAGS

```python
CFLAGS_COMMON="-O2 -pipe -mfloat-abi=hard -fno-ident -frename-registers -fexcess-precision=fast -fomit-frame-pointer -fweb"
CFLAGS_CPU="-march=armv7-a+mp+sec -mcpu=marvell-pj4 -mfpu=vfpv3"
CFLAGS_LTO="-flto -fuse-linker-plugin"

CFLAGS="${CFLAGS_COMMON} ${CFLAGS_CPU} ${CFLAGS_LTO}"
CXXFLAGS="${CFLAGS} -fvisibility-inlines-hidden"
```
### LD FLAGS

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization) and [GOLD](https://en.wikipedia.org/wiki/Gold_(linker)) linker plugin.

```python
# Linker Flags
LDFLAGS_COMMON="-Wl,--hash-style=gnu -Wl,--enable-new-dtags -Wl,-fuse-ld=gold"
LDFLAGS_LTO="-flto -fuse-linker-plugin"
LDFLAGS="${LDFLAGS_COMMON} ${LDFLAGS_LTO}"
```
