# armv4t-arm920t-linux-musleabi

Packages optimized for ARMs [ARM920T](https://en.wikipedia.org/wiki/ARM9) CPU cores.

<img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/S3C410A.png" alt="88F6282" width="160" />

These cores can be found on some of Samsungs [S3C](https://en.wikipedia.org/wiki/List_of_Samsung_system-on-a-chips) SoCs for example:

* [S3C2410](https://elinux.org/S3C2410)
* [S3C2440](https://elinux.org/S3C2440)

## Usage

Binhost can be enabled by adding these lines to the **make.conf**.

```bash
# enable binhost
PORTAGE_BINHOST="https://raw.githubusercontent.com/spreequalle/gentoo-binhost/${CHOST}"
FEATURES="${FEATURES} getbinpkg"
```

## Details

### Profile

Packages are generated using gentoo 17.0 [musl](https://www.musl-libc.org/) profile.

### USE Flags

```bash
USE="bzip2 minimal mpfr"
USE="${USE} -acl -pam -nls -ipv6 -pax_kernel -threads -pic -hardened -openmp \
-filecaps -seccomp -xattr"
```

### C FLAGS

Aggressively optimizing for size by using *-Os* and  [Thumb](http://infocenter.arm.com/help/topic/com.arm.doc.ddi0344c/Beiiegaf.html) mode.

```bash
CFLAGS_MAIN="-Os -pipe -fno-ident -frename-registers -msoft-float -fweb -fexcess-precision=fast -fomit-frame-pointer"

CFLAGS_CPU_CACHE="--param l1-cache-size=16 --param l1-cache-line-size=32"
CFLAGS_CPU="-mcpu=arm920t ${CFLAGS_CPU_CACHE}"

CFLAGS_LTO="-flto -fuse-linker-plugin"
CFLAGS="${CFLAGS_MAIN} ${CFLAGS_CPU} ${CFLAGS_LTO}"
CXXFLAGS="${CFLAGS} -fvisibility-inlines-hidden"
```
### LD FLAGS

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization) linker plugin.

```bash
LDFLAGS_MAIN="-Wl,--hash-style=gnu -Wl,--enable-new-dtags"
LDFLAGS_LTO="-flto -fuse-linker-plugin"
LDFLAGS_LD_BFD="-Wl,-fuse-ld=bfd"

LDFLAGS="${LDFLAGS_MAIN} ${LDFLAGS_LTO} ${LDFLAGS_LD_BFD}"
```
