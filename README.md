# x86_64-pc-linux-gnu


Packages optimized for Generic x86_64 AMD/Intel Processors.

<img src="https://raw.githubusercontent.com/ahnafhabib404/gentoo-binhost/master/images/648625-vermeer-black-chip-render-1260x709_2.png" alt="648625-vermeer-black-chip-render-1260x709_2" width="250" />



Example (AMD FX 8350)

```
$ lscpu
Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   48 bits physical, 48 bits virtual
CPU(s):                          8
On-line CPU(s) list:             0-7
Thread(s) per core:              2
Core(s) per socket:              4
Socket(s):                       1
NUMA node(s):                    1
Vendor ID:                       AuthenticAMD
CPU family:                      21
Model:                           2
Model name:                      AMD FX(tm)-8350 Eight-Core Processor
Stepping:                        0
Frequency boost:                 enabled
CPU MHz:                         2096.815
CPU max MHz:                     4000.0000
CPU min MHz:                     1400.0000
BogoMIPS:                        8003.06
Virtualization:                  AMD-V
L1d cache:                       64 KiB
L1i cache:                       256 KiB
L2 cache:                        8 MiB
L3 cache:                        8 MiB
NUMA node0 CPU(s):               0-7
Vulnerability Itlb multihit:     Not affected
Vulnerability L1tf:              Not affected
Vulnerability Mds:               Not affected
Vulnerability Meltdown:          Not affected
Vulnerability Spec store bypass: Mitigation; Speculative Store Bypass disabled via prctl and seccomp
Vulnerability Spectre v1:        Mitigation; usercopy/swapgs barriers and __user pointer sanitization
Vulnerability Spectre v2:        Mitigation; Full AMD retpoline, IBPB conditional, STIBP disabled, RSB filling
Vulnerability Srbds:             Not affected
Vulnerability Tsx async abort:   Not affected
Flags:                           fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fx
                                 sr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc rep_good nopl
                                  nonstop_tsc cpuid extd_apicid aperfmperf pni pclmulqdq monitor ssse3 fma cx16 sse4_1 
                                 sse4_2 popcnt aes xsave avx f16c lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a m
                                 isalignsse 3dnowprefetch osvw ibs xop skinit wdt fma4 tce nodeid_msr tbm topoext perfc
                                 tr_core perfctr_nb cpb hw_pstate ssbd ibpb vmmcall bmi1 arat npt lbrv svm_lock nrip_sa
                                 ve tsc_scale vmcb_clean flushbyasid decodeassists pausefilter pfthreshold
```
## Usage

Binhost can be enabled by adding these lines to the **make.conf**.

```bash
# enable binhost
PORTAGE_BINHOST="https://raw.githubusercontent.com/ahnafhabib404/gentoo-binhost/${CHOST}"
FEATURES="${FEATURES} getbinpkg"
```

## Details

### Profile

Packages are generated using gentoo 17.1 desktop profile.
