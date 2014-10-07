---
layout: post
title:  "Những lệnh Linux đùng để kiểm tra phần cứng"
date:   2014-10-07
tags: cmd, command line, linux, hardware, check
description: "Những lệnh Linux đùng để kiểm tra phần cứng"
---

Why? Trong trường hợp bạn là một super-sysadmin của một công ty nho nhỏ
, bạn phải lo cả những việc về phần cứng, đôi lúc bạn sẽ tiếp nhận một 
hệ thống hoàn toàn mới. Một trong các các công là phải nắm tất cả thông
tin phần cứng để có thể nên kế hoạch phân bổ tài nguyên cho hợp lý.

Tuy nhiên Server thường đặt ở Data Center, và lúc nhận Server về có thể
sysadmin cũ không lưu lại hết các thông tin về cấu hình. Việc cần làm 
là sử dụng các lệnh trên Linux để kiểm tra phần cứng.

Các thông tin:

* CPU loại gì, bao nhiêu core, tốc độ bao nhiêu, bao nhiêu CPU
* RAM hãng gì, đang gắn bao nhiêu slot, main support tối đa bao nhiêu slot
* Ổ cứng là SSD hay HDD, hãng gì, dung lượng bao nhiêu, bao nhiêu ổ, có RAID ko?
* Card mạng tốc độ bao nhiêu, main gì....

Hầu hết các lệnh đã có sẵn trên hệ thống Linux, tuy nhiên bạn sẽ cần cài thêm

```bash
	~$ yum install dmidecode
```

### 1. Tổng thể server

```bash
	~$ dmidecode -t system
	# dmidecode 2.11
	SMBIOS 2.6 present.

	Handle 0x0100, DMI type 1, 27 bytes
	System Information
	        Manufacturer: Dell Inc.
	        Product Name: PowerEdge R510
	        Version: Not Specified
	        Serial Number: 1SXRB2S
	        UUID: 4C4C4544-0053-5810-8052-B1C04F423253
	        Wake-up Type: Power Switch
	        SKU Number: Not Specified
	        Family: Not Specified

	Handle 0x0C00, DMI type 12, 5 bytes
	System Configuration Options
	        Option 1: NVRAM_CLR:  Clear user settable NVRAM areas and set defaults
	        Option 2: PWRD_EN:  Close to enable password

	Handle 0x2000, DMI type 32, 11 bytes
	System Boot Information
	        Status: No errors detected
```

=> Server Dell, Model PowerEdge R510

### 2. Kiểm tra CPU

CPU loại gì, tốc độ bao nhiêu

```bash
	~$ cat /proc/cpuinfo | grep -i 'model name' | uniq 
	model name      : Intel(R) Xeon(R) CPU E3-1230 v3 @ 3.30GHz
```

CPU có bao nhiêu core

```bash
	~$ cat /proc/cpuinfo | grep -i processor
	processor       : 0
	processor       : 1
	processor       : 2
	processor       : 3
	processor       : 4
	processor       : 5
	processor       : 6
	processor       : 7
```

Server đang cắm mấy CPU, có hỗ trợ ảo hóa hay không

```bash
	~$ lscpu 
	Architecture:          x86_64
	CPU op-mode(s):        32-bit, 64-bit
	Byte Order:            Little Endian
	CPU(s):                16
	On-line CPU(s) list:   0-15
	Thread(s) per core:    2
	Core(s) per socket:    4
	Socket(s):             2
	NUMA node(s):          2
	Vendor ID:             GenuineIntel
	CPU family:            6
	Model:                 44
	Stepping:              2
	CPU MHz:               2400.169
	BogoMIPS:              4799.93
	Virtualization:        VT-x
	L1d cache:             32K
	L1i cache:             32K
	L2 cache:              256K
	L3 cache:              12288K
	NUMA node0 CPU(s):     0,2,4,6,8,10,12,14
	NUMA node1 CPU(s):     1,3,5,7,9,11,13,15
```

Và nhiều thông tin hơn nữa

```bash
	~$ dmidecode -t 4
	# dmidecode 2.12
	SMBIOS 2.7 present.

	Handle 0x0400, DMI type 4, 42 bytes
	Processor Information
	        Socket Designation: Proc 1
	        Type: Central Processor
	        Family: Xeon
	        Manufacturer: Intel
	        ID: A9 06 03 00 FF FB EB BF
	        Signature: Type 0, Family 6, Model 58, Stepping 9
	        Flags:
	                FPU (Floating-point unit on-chip)
	                VME (Virtual mode extension)
	                DE (Debugging extension)
	                PSE (Page size extension)
	                TSC (Time stamp counter)
	                MSR (Model specific registers)
	                PAE (Physical address extension)
	                MCE (Machine check exception)
	                CX8 (CMPXCHG8 instruction supported)
	                APIC (On-chip APIC hardware supported)
	                SEP (Fast system call)
	                MTRR (Memory type range registers)
	                PGE (Page global enable)
	                MCA (Machine check architecture)
	                CMOV (Conditional move instruction supported)
	                PAT (Page attribute table)
	                PSE-36 (36-bit page size extension)
	                CLFSH (CLFLUSH instruction supported)
	                 DS (Debug store)
	                ACPI (ACPI supported)
	                MMX (MMX technology supported)
	                FXSR (FXSAVE and FXSTOR instructions supported)
	                SSE (Streaming SIMD extensions)
	                SSE2 (Streaming SIMD extensions 2)
	                SS (Self-snoop)
	                HTT (Multi-threading)
	                TM (Thermal monitor supported)
	                PBE (Pending break enabled)
	        Version:  Intel(R) Xeon(R) CPU E3-1240 V2 @ 3.40GHz      
	        Voltage: 1.4 V
	        External Clock: 100 MHz
	        Max Speed: 4800 MHz
	        Current Speed: 3400 MHz
	        Status: Populated, Enabled
	        Upgrade: Socket BGA1155
	        L1 Cache Handle: 0x0710
	        L2 Cache Handle: 0x0720
	        L3 Cache Handle: 0x0730
	        Serial Number: Not Specified
	        Asset Tag: Not Specified
	        Part Number: Not Specified
	        Core Count: 4
	        Core Enabled: 4
	        Thread Count: 8
	        Characteristics:
	        64-bit capable
```

### 3. Kiểm tra RAM

Hệ thống đang có bao nhiêu RAM

```bash
	~$ cat /proc/meminfo | head -n 2                                                                                                                                           
	MemTotal:       32707588 kB
	MemFree:        10925268 kB
```

Hệ thống đang gắn bao nhiêu slot RAM, và mỗi slot bao nhiêu

```bash
	~$ dmidecode -t 17 | grep -i size
        Size: 8192 MB
        Size: 8192 MB
        Size: 8192 MB
        Size: 8192 MB
```

=> Hệ thống có tối đa 4 slot, đang gắn 4 slot, mỗi slot 8GB

```bash
	~$ dmidecode -t 17 | grep -i size
        Size: 8192 MB
        Size: 8192 MB
        Size: No Module Installed
        Size: No Module Installed
        Size: 8192 MB
        Size: 8192 MB
        Size: No Module Installed
        Size: No Module Installed
```

=> Hệ thống gắn tối đa được 8 slot, hiện đang gắn 4 slot (0, 1, 4, 5)
mỗi slot 8GB

Thông tin chi tiết hơn về RAM

```bash
	~$ dmidecode -t 17
	Handle 0x1100, DMI type 17, 28 bytes
	Memory Device
	        Array Handle: 0x1000
	        Error Information Handle: Not Provided
	        Total Width: 72 bits
	        Data Width: 64 bits
	        Size: 8192 MB
	        Form Factor: DIMM
	        Set: 1
	        Locator: DIMM_A1 
	        Bank Locator: Not Specified
	        Type: DDR3
	        Type Detail: Synchronous Registered (Buffered)
	        Speed: 1333 MHz
	        Manufacturer: 063404B30000
	        Serial Number: 00000000
	        Asset Tag: 05000011
	        Part Number: SUPERTALENT02     
	        Rank: 4
```

=> như ta thấy ở đây ta biết được Speed, Serial Number, Part Number, Manufacturer
