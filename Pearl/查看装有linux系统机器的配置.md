---
title: 查看装有linux系统机器的配置
date: 2016-11-30 23:27:40
tags: [Linux,Pearl]
---

<br>

### <font color="DeepSkyBlue">查看内存</font>

<br>


<font color="red">`free -m` </font>: 以M为单位进行展示内存使用情况<br>

```shell
              total        used        free      shared  buff/cache   available
Mem:           3951        1797         910           4        1243        1853
Swap:             0
```


`free -g` : 以G为单位进行展示内存使用情况

```
              total        used        free      shared  buff/cache   available
Mem:              3           1           0           0           1           1
Swap:             0           0           0
```

以G为单位且"退一法"策略,精度不够准确

<br>

---


### <font color="DeepSkyBlue">查看系统版本号</font>

<br>

`cat /etc/issue`

```
Ubuntu 16.04.3 LTS \n \l
```

---


### <font color="DeepSkyBlue">查看CPU主频信息</font>

<br>

`lscpu`

```
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                2
On-line CPU(s) list:   0,1
Thread(s) per core:    2
Core(s) per socket:    1
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 79
Model name:            Intel(R) Xeon(R) CPU E5-2682 v4 @ 2.50GHz
Stepping:              1
CPU MHz:               2494.222
BogoMIPS:              4988.44
Hypervisor vendor:     KVM
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              40960K
NUMA node0 CPU(s):     0,1
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm rdseed adx smap xsaveopt
```

---


### <font color="DeepSkyBlue">查看系统的ip地址</font>

<br>

`ifconfig `

```
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
	options=1203<RXCSUM,TXCSUM,TXSTATUS,SW_TIMESTAMP>
	inet 127.0.0.1 netmask 0xff000000
	inet6 ::1 prefixlen 128
	inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1
	nd6 options=201<PERFORMNUD,DAD>
gif0: flags=8010<POINTOPOINT,MULTICAST> mtu 1280
stf0: flags=0<> mtu 1280
XHC1: flags=0<> mtu 0
XHC20: flags=0<> mtu 0
XHC0: flags=0<> mtu 0
en6: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether ac:de:48:00:11:22
	inet6 fe80::aede:48ff:fe00:1122%en6 prefixlen 64 scopeid 0x7
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether 78:4f:43:51:7c:f3
	inet 15.38.229.23 netmask 0xfffff800 broadcast 15.38.231.255
	media: autoselect
	status: active
p2p0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 2304
	ether 0a:4f:43:51:7c:f3
	media: autoselect
	status: inactive
awdl0: flags=8943<UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1484
	ether fa:83:12:04:95:ed
	inet6 fe80::f883:12ff:fe04:95ed%awdl0 prefixlen 64 scopeid 0xa
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
en1: flags=8963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
	options=60<TSO4,TSO6>
	ether ee:00:78:72:c1:01
	media: autoselect <full-duplex>
	status: inactive
en2: flags=8963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
	options=60<TSO4,TSO6>
	ether ee:00:78:72:c1:00
	media: autoselect <full-duplex>
	status: inactive
en3: flags=8963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
	options=60<TSO4,TSO6>
	ether ee:00:78:72:c1:05
	media: autoselect <full-duplex>
	status: inactive
en4: flags=8963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
	options=60<TSO4,TSO6>
	ether ee:00:78:72:c1:04
	media: autoselect <full-duplex>
	status: inactive
bridge0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=63<RXCSUM,TXCSUM,TSO4,TSO6>
	ether ee:00:78:72:c1:01
	Configuration:
		id 0:0:0:0:0:0 priority 0 hellotime 0 fwddelay 0
		maxage 0 holdcnt 0 proto stp maxaddr 100 timeout 1200
		root id 0:0:0:0:0:0 priority 0 ifcost 0 port 0
		ipfilter disabled flags 0x2
	member: en1 flags=3<LEARNING,DISCOVER>
	        ifmaxaddr 0 port 11 priority 0 path cost 0
	member: en2 flags=3<LEARNING,DISCOVER>
	        ifmaxaddr 0 port 12 priority 0 path cost 0
	member: en3 flags=3<LEARNING,DISCOVER>
	        ifmaxaddr 0 port 13 priority 0 path cost 0
	member: en4 flags=3<LEARNING,DISCOVER>
	        ifmaxaddr 0 port 14 priority 0 path cost 0
	media: <unknown type>
	status: inactive
utun0: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 2000
	inet6 fe80::d73b:4182:e869:9982%utun0 prefixlen 64 scopeid 0x10
	nd6 options=201<PERFORMNUD,DAD>
```


---


### <font color="DeepSkyBlue">查看系统的CPU详细信息</font>

<br>

`cat /proc/cpuinfo`

```
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 79
model name	: Intel(R) Xeon(R) CPU E5-2682 v4 @ 2.50GHz
stepping	: 1
microcode	: 0x1
cpu MHz		: 2494.222
cache size	: 40960 KB
physical id	: 0
siblings	: 2
core id		: 0
cpu cores	: 1
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm rdseed adx smap xsaveopt
bugs		:
bogomips	: 4988.44
clflush size	: 64
cache_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:

processor	: 1
vendor_id	: GenuineIntel
cpu family	: 6
model		: 79
model name	: Intel(R) Xeon(R) CPU E5-2682 v4 @ 2.50GHz
stepping	: 1
microcode	: 0x1
cpu MHz		: 2494.222
cache size	: 40960 KB
physical id	: 0
siblings	: 2
core id		: 0
cpu cores	: 1
apicid		: 1
initial apicid	: 1
fpu		: yes
fpu_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm rdseed adx smap xsaveopt
bugs		:
bogomips	: 4988.44
clflush size	: 64
cache_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
```


---


