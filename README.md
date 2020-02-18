# aarch64-cortex-linux-gnu

Packages optimized for Pine64's Rock64/pro , Pinebook Pro & RPI3/4
https://isshoni.org/pi64pie While they have a sysvinit/openrc profile I've been building for Systemd
and added packages ie from [Pentoo|github.com/pentoo] packages ports 
https://github.com/sakaki-/gentoo-on-rpi-64bit 
A few other repos are used for Rock64/Pinebook Pro 

<img src="https://raw.githubusercontent.com/sakaki-/resources/master/raspberrypi/pi4/Raspberry_Pi_3_B_and_B_plus_and_4_B.jpg" alt="88F6282" width="160" />
https://www.youtube.com/watch?v=9CCQicHwfDI some of the Rockpro64 Like PIC-E4x and Sata or M.2 , ASUS has a 4x slot nvme card


##### Still Editing  and Migrating over from fokred template.
--------------------------
These cores can be found on the Cavium (later Marvell) [ThunderX](https://web.archive.org/web/20190131010413/https://www.marvell.com/server-processors/thunderx-arm-processors/) SoCs for example:

* CN8890

```
lscpu
Architecture:        aarch64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              6
On-line CPU(s) list: 0-5
Thread(s) per core:  1
Core(s) per socket:  3
Socket(s):           2
Vendor ID:           ARM
Model:               4
Model name:          Cortex-A53
Stepping:            r0p4
CPU max MHz:         1992.0000
CPU min MHz:         408.0000
BogoMIPS:            48.00
Flags:               fp asimd evtstrm aes pmull sha1 sha2 crc32

cpuid2cpuflags
CPU_FLAGS_ARM: edsp neon thumb vfp vfpv3 vfpv4 vfp-d32 aes sha1 sha2 crc32 v4 v5 v6 v7 v8 thumb2
#@rock64pro 
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
*cortex-a72.cortex-a53 is common to RPI4 RPI3 backwards compatible , as well as workable with Rock64 Rockpro64 & Pinebook Pro*

```python
COMMON_FLAGS="-O2 -pipe -march=armv8-a+crc+crypto -mcpu=cortex-a72.cortex-a53"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
```
```# Graphite-specific CFLAGS #optional graphite flags.
CFLAGS="${COMMON_FLAGS} ${GRAPHITE}"
GRAPHITE="-floop-interchange -ftree-loop-distribution -floop-strip-mine -floop-block"
````  
### LD FLAGS

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization).

```python
LDFLAGS_COMMON="-Wl,--hash-style=gnu -Wl,--enable-new-dtags -Wl,-fuse-ld=bfd"
LDFLAGS_LTO="-flto -fuse-linker-plugin"
LDFLAGS="${LDFLAGS_COMMON} ${LDFLAGS_LTO}"
```
