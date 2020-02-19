# aarch64-cortex_a53-linux-gnu

Packages optimized for ARMs [Cortex-A53](https://en.wikipedia.org/wiki/ARM_Cortex-A53) cores.

<img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/BCM2837.png" alt="S5L8900" width="160" />

These cores can be found on many SoCs like the  Broadcom [BCM2837](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2837/README.md) or other vendors like Qualcomm, Rockchip.

```
$ lscpu
Architecture:        aarch64
Byte Order:          Little Endian
CPU(s):              4
On-line CPU(s) list: 0-3
Thread(s) per core:  1
Core(s) per socket:  4
Socket(s):           1
Vendor ID:           ARM
Model:               4
Model name:          Cortex-A53
Stepping:            r0p4
CPU max MHz:         1400.0000
CPU min MHz:         600.0000
BogoMIPS:            38.40
Flags:               fp asimd evtstrm crc32 cpuid
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
USE="-nls -tcpd -alsa -cups -gtk"
USE="${USE} lz4 lzma lzo curl gmp neon threads elf"
```

### C FLAGS

```bash
CFLAGS_COMMON="-O2 -pipe -fomit-frame-pointer -fno-ident"
CFLAGS_CPU="-mcpu=cortex-a53+crc"
CFLAGS_LTO="-flto -fuse-linker-plugin"

CFLAGS="${CFLAGS_COMMON} ${CFLAGS_CPU} ${CFLAGS_LTO}"
CXXFLAGS="${CFLAGS} -fvisibility-inlines-hidden"
```
### LD FLAGS

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization).

```bash
LDFLAGS_COMMON="-Wl,--hash-style=gnu -Wl,--enable-new-dtags -Wl,-fuse-ld=bfd"
LDFLAGS_LTO="-flto -fuse-linker-plugin"
LDFLAGS="${LDFLAGS_COMMON} ${LDFLAGS_LTO}"
```

