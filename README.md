# armv6kz-arm1176jzf-linux-gnueabihf

Packages optimized for ARMs [ARM1176](https://en.wikipedia.org/wiki/ARM11) cores with integrated [FPU](https://en.wikipedia.org/wiki/Floating-point_unit).

<img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/S5L8900.png" alt="S5L8900" width="160" />

These cores can be found on the Apple [S5L8900](https://en.wikipedia.org/wiki/Apple-designed_processors#Early_series) or Broadcom [BCM2835](https://web.archive.org/web/20120513032855/http://www.broadcom.com/products/BCM2835) SoCs.

```
$ lscpu
Architecture:        armv6l
Byte Order:          Little Endian
CPU(s):              1
On-line CPU(s) list: 0
Thread(s) per core:  1
Core(s) per socket:  1
Socket(s):           1
Vendor ID:           ARM
Model:               7
Model name:          ARM1176
Stepping:            r0p7
CPU max MHz:         700,0000
CPU min MHz:         700,0000
BogoMIPS:            697.95
Flags:               half thumb fastmult vfp edsp java tls
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

Packages are generated using gentoo 17.0 [armv6j](https://wiki.gentoo.org/wiki/Project:ARM) profile.

### USE Flags

```bash
USE="-acl -cups -nls -openmp -tcpd -xattr"
USE="${USE} curl gmp lz4 lzma lzo systemd"
```

### C FLAGS

```bash
CFLAGS_COMMON="-O2 -pipe -fomit-frame-pointer -fno-ident -frename-registers \
-fexcess-precision=fast -fweb"
CFLAGS_CPU="-march=armv6kz+fp -mcpu=arm1176jzf-s -mfpu=vfp -mfloat-abi=hard \
--param l1-cache-size=16 --param l1-cache-line-size=32 \
--param l2-cache-size=128"
CFLAGS_LTO="-flto -fuse-linker-plugin"

CFLAGS="${CFLAGS_COMMON} ${CFLAGS_CPU} ${CFLAGS_LTO}"
CXXFLAGS="${CFLAGS} -fvisibility-inlines-hidden"
```

### LD FLAGS

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization) and [GOLD](https://en.wikipedia.org/wiki/Gold_(linker)) linker plugin.

```bash
LDFLAGS_COMMON="-Wl,--hash-style=gnu -Wl,--enable-new-dtags -Wl,-fuse-ld=gold"
LDFLAGS_LTO="-flto -fuse-linker-plugin"
LDFLAGS="${LDFLAGS_COMMON} ${LDFLAGS_LTO}"
```
