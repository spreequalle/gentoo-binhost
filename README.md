# x86_64-bonnell-linux-gnu

Packages optimized for 64bit Intel [Bonnell](https://en.wikipedia.org/wiki/Bonnell_(microarchitecture)) based cores.

<img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/Atom_N270_diamondville.png" alt="88F6282" width="160" />

These cores can be found on the Intel Diamondville Atom CPUs, for example:

* Atom 230
* Atom 330

```
$ lscpu
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
Address sizes:       32 bits physical, 48 bits virtual
CPU(s):              4
On-line CPU(s) list: 0-3
Thread(s) per core:  2
Core(s) per socket:  2
Socket(s):           1
Vendor ID:           GenuineIntel
CPU family:          6
Model:               28
Model name:          Intel(R) Atom(TM) CPU  330   @ 1.60GHz
Stepping:            2
CPU MHz:             1999.922
BogoMIPS:            3999.84
L1d cache:           24K
L1i cache:           32K
L2 cache:            512K
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx lm constant_tsc arch_perfmon pebs bts nopl cpuid aperfmperf pni dtes64 monitor ds_cpl tm2 ssse3 cx16 xtpr pdcm movbe lahf_lm dtherm
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

Packages are generated using gentoo 17.1 no-[multilib](https://wiki.gentoo.org/wiki/Multilib) profile.

### USE Flags

```bash
USE="X alsa bindist bluetooth cdio cjk dbus drm dvd icu ios jpeg lcms lto lz4 lzma lzo openssl policykit samba sqlite systemd threads truetype upnp upower vdpau xkb zstd"
USE="${USE} -cups -gtk -introspection -ldap -nls -numa -spell -static"
```

### C FLAGS

Optimized for bonnel [in-order](https://en.wikipedia.org/wiki/Out-of-order_execution) architecture.

```bash
CFLAGS_COMMON="-O2 -pipe -fno-ident -fexcess-precision=fast -fomit-frame-pointer"
CFLAGS_CACHE="--param l1-cache-size=24 --param l1-cache-line-size=64 --param l2-cache-size=512"
CFLAGS_CPU="-march=bonnell -mtune=bonnell -mmmx -mno-3dnow -msse -msse2 -msse3 -mssse3 -mno-sse4a -mcx16 -msahf -mmovbe -mno-aes -mno-sha -mno-pclmul -mno-popcnt -mno-abm -mno-lwp -mno-fma -mno-fma4 -mno-xop -mno-bmi -mno-sgx -mno-bmi2 -mno-pconfig -mno-wbnoinvd -mno-tbm -mno-avx -mno-avx2 -mno-sse4.2 -mno-sse4.1 -mno-lzcnt -mno-rtm -mno-hle -mno-rdrnd -mno-f16c -mno-fsgsbase -mno-rdseed -mno-prfchw -mno-adx -mfxsr -mno-xsave -mno-xsaveopt -mno-avx512f -mno-avx512er -mno-avx512cd -mno-avx512pf -mno-prefetchwt1 -mno-clflushopt -mno-xsavec -mno-xsaves -mno-avx512dq -mno-avx512bw -mno-avx512vl -mno-avx512ifma -mno-avx512vbmi -mno-avx5124fmaps -mno-avx5124vnniw -mno-clwb -mno-mwaitx -mno-clzero -mno-pku -mno-rdpid -mno-gfni -mno-shstk -mno-avx512vbmi2 -mno-avx512vnni -mno-vaes -mno-vpclmulqdq -mno-avx512bitalg -mno-movdiri -mno-movdir64b -mno-waitpkg -mno-cldemote -mno-ptwrite ${CFLAGS_CACHE}"

CFLAGS_LTO="-flto -fuse-linker-plugin"

CFLAGS="${CFLAGS_COMMON} ${CFLAGS_CPU} ${CFLAGS_LTO}"
CXXFLAGS="${CFLAGS} -fvisibility-inlines-hidden"
```
### LD FLAGS

Enable system-wide [LTO](https://gcc.gnu.org/wiki/LinkTimeOptimization) and [GOLD](https://en.wikipedia.org/wiki/Gold_(linker)) linker plugin.

```bash
LDFLAGS_MAIN="-Wl,--hash-style=gnu -Wl,--enable-new-dtags"
LDFLAGS_LTO="-flto -fuse-linker-plugin"

LDFLAGS_LD_BFD="-Wl,-fuse-ld=bfd"
LDFLAGS_LD_GOLD="-Wl,-fuse-ld=gold"

LDFLAGS="${LDFLAGS_MAIN} ${LDFLAGS_LTO} ${LDFLAGS_LD_GOLD}"
```
