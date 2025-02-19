The Memory Process File System:
===============================
The Memory Process File System (MemProcFS) is an easy and convenient way of viewing physical memory as files in a virtual file system. 

Easy trivial point and click memory analysis without the need for complicated commandline arguments! Access memory content and artifacts via files in a mounted virtual file system or via a feature rich application library to include in your own projects!

Analyze memory dump files, <b>live memory</b> via DumpIt or WinPMEM, <b>live memory in read-write mode</b> via linked [PCILeech](https://github.com/ufrisk/pcileech/) and [PCILeech-FPGA](https://github.com/ufrisk/pcileech-fpga/) devices!

It's even possible to connect to a remote LeechAgent memory acquisition agent over a secured connection - allowing for remote live memory incident response - even over higher latency low band-width connections!

Use your favorite tools to analyze memory - use your favorite hex editors, your python and powershell scripts, WinDbg or your favorite disassemblers and debuggers - all will work trivally with MemProcFS by just reading and writing files!

<p align="center"><img src="https://github.com/ufrisk/MemProcFS/wiki/resources/proc_base3.png" height="190"/><img src="https://github.com/ufrisk/MemProcFS/wiki/resources/pciescreamer.jpeg" height="190"/><img src="https://github.com/ufrisk/MemProcFS/wiki/resources/proc_modules.png" height="190"/></p>


Include MemProcFS in your Python or C/C++ programming projects! Everything in MemProcFS is exposed via an easy-to-use API for use in your own projects! The Plugin friendly architecture allows users to easily extend MemProcFS with native C .DLL plugins or Python .py plugins - providing additional analysis capabilities!

<b>Please check out the [project wiki](https://github.com/ufrisk/MemProcFS/wiki)</b> for more in-depth detailed information about the file system itself, its API and its plugin modules!

<b>Please check out the [LeechCore project](https://github.com/ufrisk/LeechCore)</b> for information about supported memory acquisition methods and remote memory access via the LeechService.

To get going clone the sources in the repository or download the [latest binaries, modules and configuration files](https://github.com/ufrisk/MemProcFS/releases/latest) from the releases section and **check out the [guide](https://github.com/ufrisk/MemProcFS/wiki).**

Fast and easy memory analysis via mounted file system:
======================================================
No matter if you have no prior knowledge of memory analysis or are an advanced user MemProcFS (and its API) may be useful! Click around the memory objects in the file system

<p align="center"><img src="https://github.com/ufrisk/MemProcFS/wiki/resources/proc_procstruct.png" height="225"/><img src="https://github.com/ufrisk/MemProcFS/wiki/resources/proc_virt2phys.png" height="225"/></p>

Extensive Python and C/C++ API:
===============================
Everything in MemProcFS is exposed as APIs. APIs exist for both C/C++ `vmmdll.h` and Python `vmmpy.py`. The file system itself is made available virtually via the API without the need to mount it. Specialized process analysis and process alteration functionality is made easy by calling API functionality. It is possible to read both virtual process memory as well as physical memory! The example below shows reading 0x20 bytes from physical address 0x1000:
```
>>> from vmmpy import *
>>> VmmPy_Initialize('c:/temp/win10_memdump.raw')
>>> print(VmmPy_UtilFillHexAscii(VmmPy_MemRead(-1, 0x1000, 0x20)))
0000    e9 4d 06 00 01 00 00 00  01 00 00 00 3f 00 18 10   .M..........?...
0010    00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ................
```

Modular Plugin Architecture:
============================
Anyone is able to extend MemProcFS with custom plugins! It is as easy as dropping a python file in the correct directory or compiling a tiny C DLL. Existing functionality is already implemented as well documented C and Python plugins!

Installing:
===========
## Windows
<b>Get the latest [binaries, modules and configuration files](https://github.com/ufrisk/MemProcFS/releases/latest) from the latest release.</b> Alternatively clone the repository and build from source.

MemProcFS is dependent on the [LeechCore project](https://github.com/ufrisk/LeechCore) for memory acquisition. The necessary _leechcore.dll_ file is already pre-built and included together with the pre-built binaries.

MemProcFS is also dependent in the <b>Microsoft Visual C++ Redistributables for Visual Studio 2019</b>. They can be downloaded from Microsoft [here](https://go.microsoft.com/fwlink/?LinkId=746572). Alternatively, if installing the Dokany file system driver please install the <b>DokanSetup_redist</b> version and it will install the required redistributables.

Mounting the file system requires the <b>Dokany file system library</b> to be installed. Please download and install the latest version of Dokany at: https://github.com/dokan-dev/dokany/releases/latest It is recommended to download and install the <b>DokanSetup_redist</b> version.

Python support requires Python 3.6 or later. The user may specify the path to the Python installation with the command line parameter `-pythonhome`, alternatively download [Python 3.7 - Windows x86-64 embeddable zip file](https://www.python.org/downloads/windows/) and unzip its contents into the `files/python` folder when using Python modules in the file system. To use the Python API a normal 64-bit Python 3.6 or later installation for Windows is required.

To capture live memory (without PCILeech FPGA hardware) download [DumpIt](https://www.comae.com/) and start MemProcFS via DumpIt /LIVEKD mode. Alternatively, get WinPMEM by downloading the most recent signed [WinPMEM driver](https://github.com/Velocidex/c-aff4/tree/master/tools/pmem/resources/winpmem) and place it alongside MemProcFS - detailed instructions in the [LeechCore Wiki](https://github.com/ufrisk/LeechCore/wiki/Device_WinPMEM).

PCILeech FPGA will require hardware as well as _FTD3XX.dll_ to be dropped alongside the MemProcFS binaries. Please check out the [LeechCore](https://github.com/ufrisk/LeechCore) project for instructions.

## Linux
The memory process file system is only supported on Windows.

Examples:
=========
Start MemProcFS from the command line - possibly by using one of the examples below.

Or register the memory dump file extension with MemProcFS.exe so that the file system is automatically mounted when double-clicking on a memory dump file!

- mount the memory dump file as default M: <br>`memprocfs.exe -device c:\temp\win10x64-dump.raw`
- mount the memory dump file as default M: with extra verbosity: <br>`memprocfs.exe -device c:\temp\win10x64-dump.raw -v`
- mount the memory dump file as default M: with extra extra verbosity: <br>`memprocfs.exe -device c:\temp\win10x64-dump.raw -v -vv`
- mount the memory dump file as S: <br>`memprocfs.exe -mount s -device c:\temp\win10x64-dump.raw`
- mount live target memory, in verbose read-only mode, with DumpIt in /LIVEKD mode: <br>`DumpIt.exe /LIVEKD /A memprocfs.exe /C "-v"`
- mount live target memory, in read-only mode, with WinPMEM driver: <br>`memprocfs.exe -device pmem`
- mount live target memory, in read/write mode, with PCILeech FPGA memory acquisition device: <br>`memprocfs.exe -device fpga -memmap auto`
- mount live target memory, in read/write mode, with TotalMeltdown vulnerability acquisition device: <br>`memprocfs.exe -device totalmeltdown`
- mount a memory dump with a corresponding page files: <br>`memprocfs.exe -device unknown-x64-dump.raw -pagefile0 pagefile.sys -pagefile1 swapfile.sys`

Documentation:
==============
For additional documentation please check out the [project wiki](https://github.com/ufrisk/MemProcFS/wiki) for in-depth detailed information about the file system itself, its API and its plugin modules! For additional information about memory acqusition methods check out the [LeechCore project](https://github.com/ufrisk/LeechCore/)

Also check out my Microsoft BlueHatIL 2019 talk _Practical Uses for Hardware-assisted Memory Visualization_ and my Disobey 2020 talk _Live Memory Attacks and Forensics_ about MemProcFS.

<p align="center"><a href="https://www.youtube.com/watch?v=Da_9SV9FA34" alt="Microsoft BlueHatIL 2019 talk - Practical Uses for Hardware-assisted Memory Visualization" target="_new"><img src="http://img.youtube.com/vi/Da_9SV9FA34/0.jpg" height="250"/></a> <a href="https://youtu.be/mca3rLsHuTA?t=952" alt="Disobey 2020 talk - Live Memory Attacks and Forensics" target="_new"><img src="http://img.youtube.com/vi/mca3rLsHuTA/0.jpg" height="250"/></a></p>


Building:
=========
<b>Pre-built [binaries, modules and configuration files](https://github.com/ufrisk/MemProcFS/releases/latest) are found in the latest release.</b>. MemProcFS binaries are built with Visual Studio. MemProcFS is not supported on Linux.

Detailed build instructions may be found in the [Wiki](https://github.com/ufrisk/MemProcFS/wiki) in the [Building](https://github.com/ufrisk/MemProcFS/wiki/Dev_Building) section.

Current Limitations & Future Development:
=========================================
MemProcFS is currently limited to analyzing Windows (32-bit and 64-bit XP to 10) memory dumps.

Please find some ideas for possible future expansions of the memory process file system listed below. This is a list of ideas - not a list of features that will be implemented. Even though some items are put as prioritized there is no guarantee that they will be implemented in a timely fashion.

### Prioritized items:
- More/new plugins.

### Other items:
- Hash lookup of executable memory pages in DB.
- Forensic mode more more analysis tasks.
- Forensic mode JSON file generation.

License:
======
The project source code is released under GPLv3. Some bundled Microsoft redistributable binaries are released under separate licenses.

Contributing:
=============
PCILeech, MemProcFS and LeechCore are open source but not open contribution. PCILeech, MemProcFS and LeechCore offers a highly flexible plugin architecture that will allow for contributions in the form of plugins. If you wish to make a contribution, other than a plugin, to the core projects please contact me before starting to develop.

Links:
======
* Twitter: [![Twitter](https://img.shields.io/twitter/follow/UlfFrisk?label=UlfFrisk&style=social)](https://twitter.com/intent/follow?screen_name=UlfFrisk)
* Discord: [![Discord | Porchetta Industries](https://img.shields.io/discord/736724457258745996.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2)](https://discord.gg/sEkn3aa)
* PCILeech: https://github.com/ufrisk/pcileech
* PCILeech FPGA: https://github.com/ufrisk/pcileech-fpga
* LeechCore: https://github.com/ufrisk/LeechCore
* MemProcFS: https://github.com/ufrisk/MemProcFS
* YouTube: https://www.youtube.com/channel/UC2aAi-gjqvKiC7s7Opzv9rg
* Blog: http://blog.frizk.net

Support PCILeech/MemProcFS development:
=======================================
PCILeech and MemProcFS are hobby projects of mine. I put a lot of time and energy into my projects. The time being most of my spare time - since I'm not able to work with this. Unfortunately since some aspects also relate to hardware I also put quite some of money into my projects. If you think PCILeech and/or MemProcFS are awesome tools and/or if you had a use for them it's now possible to contribute.

Please do note that PCILeech and MemProcFS are free and open source - as such I'm not expecting sponsorships; even though a sponsorship would be very much appreciated. I'm also not able to promise product features, consultancy or other things in return for a donation. A sponsorship will have to stay a sponsorship and no more. It's possible to sponsor via Github Sponsors (preferred way), but also via PayPal or Bitcoin.

 - Github Sponsors: [`https://github.com/sponsors/ufrisk`](https://github.com/sponsors/ufrisk) (preferred)
 - Paypal: `paypal@ulffrisk.com` 
 - Bitcoin: `bc1q9kur5pym8wmh5yxkf65792rdqm0guncd2gl4tu`

Changelog:
===================
v1.0
* Initial Release.

v1.1-v2.10
* Various updates. Please see individual relases for more information.

[v3.0](https://github.com/ufrisk/MemProcFS/releases/tag/v3.0)
* Major release with new features, optimizations and refactorings.
* New virtual memory core for increased speed and memory recovery:
  * VAD (virtual address descriptor) support.
  * Win10 memory decompression bug-fixes.
  * Pagefile support.
* Handles.
* Threads.
* API: new features and updates (module names from ansi to wide string).

[v3.1](https://github.com/ufrisk/MemProcFS/releases/tag/v3.1)
* Bug fixes and refactorings.
* Code signing of binaries.
* New Features:
  * Users.
  * Volatile registry keys.
  * File recovery via Handles and Vads.  

[v3.2](https://github.com/ufrisk/MemProcFS/releases/tag/v3.2)
* Bug fixes.
* Support for low-memory x64 systems.
* New Features:
  * Certificates.
  * Physical memory map.
  * Per-page physical memory information (PFN database).
  * Registry "big data" value type support.

[v3.3](https://github.com/ufrisk/MemProcFS/releases/tag/v3.3)
* Bug fixes.
* Better write support.
* AMD Ryzen FPGA support.
* Module map: new info - Full .DLL Path.
* Thread map: new info - CPU registers.
* New forensic mode:
  * Timelining.
  * NTFS MFT parsing.
  * SQLITE database generation.
* New Features:
  * Minidump .DMP file generation for individual processes.
  * Syscalls - nt & win32k.
