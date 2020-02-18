# aarch64-cn8890-linux-gnu

Packages optimized for Cavium ThunderX cores.

<img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/CN8890.png" alt="88F6282" width="160" />

These cores can be found on the Cavium (later Marvell) [ThunderX](https://web.archive.org/web/20190131010413/https://www.marvell.com/server-processors/thunderx-arm-processors/) SoCs for example:

* CN8890

```
lscpu
Architecture:        aarch64
Byte Order:          Little Endian
CPU(s):              4
On-line CPU(s) list: 0-3
Thread(s) per core:  1
Core(s) per socket:  4
Socket(s):           1
Vendor ID:           Cavium
Model:               1
Model name:          ThunderX 88XX
Stepping:            0x1
BogoMIPS:            200.00
Flags:               fp asimd evtstrm aes pmull sha1 sha2 crc32 cpuid
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

Packages are generated using gentoo 17.0 systemd [glibc](https://www.gnu.org/software/libc/) profile.

### USE Flags

```python
USE="-nls -tcpd -alsa -cups -gtk"
USE="${USE} lz4 lzma lzo curl gmp neon threads elf"
```

### C FLAGS

```python
CFLAGS_COMMON="-O2 -pipe -fomit-frame-pointer -fno-ident"
CFLAGS_CPU="-mcpu=thunderxt88+aes+sha2"
CFLAGS_LTO="-flto -fuse-linker-plugin"
CFLAGS="${CFLAGS_COMMON} ${CFLAGS_CPU} ${CFLAGS_LTO}"
CXXFLAGS="${CFLAGS} -fvisibility-inlines-hidden"
```
### LD FLAGS

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization).

```python
LDFLAGS_COMMON="-Wl,--hash-style=gnu -Wl,--enable-new-dtags -Wl,-fuse-ld=bfd"
LDFLAGS_LTO="-flto -fuse-linker-plugin"
LDFLAGS="${LDFLAGS_COMMON} ${LDFLAGS_LTO}"
```
