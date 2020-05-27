# mipsel-mips32r2el-linux-gnu

Packages optimized for [MIPS32R2](https://en.wikichip.org/wiki/mips/mips32_instruction_set#Release_2) based cores.

<img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/JZ4780.png" alt="A330" width="160" />

These cores can be found on various SoCs like the Ingenics [JZ4780](http://www.ingenic.com.cn/en/?product/id/13.html).

```
$ cat /proc/cpuinfo 
system type             : JZ4780
machine                 : img,ci20
processor               : 0
cpu model               : Ingenic JZRISC V4.15  FPU V0.0
BogoMIPS                : 1196.85
wait instruction        : yes
microsecond timers      : no
tlb_entries             : 32
extra interrupt vector  : yes
hardware watchpoint     : yes, count: 1, address/irw mask: [0x0fff]
isa                     : mips1 mips2 mips32r1 mips32r2
ASEs implemented        :
shadow register sets    : 1
kscratch registers      : 0
package                 : 0
core                    : 0
VCED exceptions         : not available
VCEI exceptions         : not available
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

Packages are generated using gentoo 17.0 no-[multilib](https://wiki.gentoo.org/wiki/Multilib) profile.

### USE Flags

```bash
USE="dbus systemd udev alsa lzma sqlite clang lto"
USE="${USE} -nls -seccomp -introspection -static -acl -debug -vala"
```

### C FLAGS

Optimized for bonnel [in-order](https://en.wikipedia.org/wiki/Out-of-order_execution) architecture.

```bash
CFLAGS_MAIN="-O2 -pipe -fno-ident -fexcess-precision=fast -fomit-frame-pointer"
CFLAGS_CACHE="--param l1-cache-size=32 --param l1-cache-line-size=32 --param l2-cache-size=256"
CFLAGS_CPU="-march=mips32r2 -mabi=32 -mfp32 -mplt -ffp-contract=off ${CFLAGS_CACHE}"
CFLAGS_LTO="-flto -fuse-linker-plugin"
CFLAGS="${CFLAGS_MAIN} ${CFLAGS_CPU} ${CFLAGS_LTO}"
CXXFLAGS="${CFLAGS} -fvisibility-inlines-hidden"
```

### LD FLAGS

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization) and [GOLD](https://en.wikipedia.org/wiki/Gold_(linker)) linker plugin.

```bash
LDFLAGS_MAIN="-Wl,--enable-new-dtags"
LDFLAGS_LTO="-flto -fuse-linker-plugin"
LDFLAGS_GOLD="-Wl,-fuse-ld=gold"
LDFLAGS="${LDFLAGS_MAIN} ${LDFLAGS_GOLD} ${LDFLAGS_LTO}"
```
