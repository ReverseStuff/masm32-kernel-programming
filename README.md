# masm32-kernel-programming

Dreg's repo for his own needs

masm32 kernel programming, drivers, tutorials, examples, and tools (credits Four-F)

# Instructions

- download/clone this repo
- run masm32v11r_install.exe
- click on install image (earth)
- install masm32 in C:\
- I recommend add to PATH environment variable following entries:
  - C:\masm32\bin
  - C:\masm32
- copy KmdKit18\include\ content to C:\masm32\include\
- copy KmdKit18\macros\ content to C:\\masm32\macros\
- copy KmdKit18\lib\ content to C:\masm32\lib\

# Basic example

- copy beeper.asm to C:\
- copy beeper.bat to C:\
- run beeper.bat

At this moment a .sys is created in C:\

# How to load/unload a driver 

## w2k_load.exe - best way

load driver:
- w2k_load.exe C:\beeper.sys

w2k_load.exe or osrloader.exe gives an error loading beeper.sys, just ignore it.

This error is generated by driver code itself when it finish its own execution (with this trick you dont need unload it)
```
mov eax, STATUS_DEVICE_CONFIGURATION_ERROR
ret
```

So, if beep.sys is correctly loaded in kernel you should listen a beep in your speakers (driver code generates a beep)

When you need unload a driver use:
- w2k_load.exe PATH_SYS_FILE /unload

I modified w2k_load.exe by Sven B. Schreiber to work for windows 2000 to windows 10 (manifest...)

## osrloader

unzip osrloaderv30.zip and use this GUI program:
- Select a .sys file (driver path)
- Register service 
- Start service

## How to view debug msg without windbg

for **older windows** just use DbgView.exe attached in this repo. Btw, I modified it (manifest...)

For modern windows use the last dbgview.exe form sysinternal web site, and check this for troubleshooting:

- https://stackoverflow.com/questions/12494300/no-output-from-debugview
- http://msdn.microsoft.com/en-us/library/windows/hardware/ff551519(v=vs.85).aspx
- https://learn.microsoft.com/en-us/sysinternals/downloads/debugview

# Next steps

Read .pdf files in this repo and look examples at [KmdKit18/examples](KmdKit18/examples) directory

**WARNING**: ALL source ASM code are inside .bat files (batch + asm code mixed in the same file)

Example with advanced\FindShadowTable\FindShadowTable.bat
```
;@echo off
;goto make

ASM DRIVER CODE GOES HERE

:make
set drv=FindShadowTable
\masm32\bin\ml /nologo /c /coff %drv%.bat
\masm32\bin\link /nologo /driver /base:0x10000 /align:32 /out:%drv%.sys /subsystem:native %drv%.obj
del %drv%.obj
echo.
pause
```

This file contains BATCH code which generates a .sys and also contains the own driver source code, 
So, yes, very tricky, the build script and source code are together

## advanced/FindShadowTable
[advanced/FindShadowTable](KmdKit18/examples/advanced/FindShadowTable)

This is an example how to find ServiceDescriptorTableShadow.

It just scans threads whith ID 80h-400h trying to find a thread which KTHREAD.ServiceTable not equal to KeServiceDescriptorTable. If such a thread is found its KTHREAD.ServiceTable holds ServiceDescriptorTableShadow address.

Watch driver's debug output with the DbgView (www.sysinternals.com) or use SoftICE.

Tested under: Windows 2000, XP and Server 2003.

## advanced/KbdSpy

[advanced/KbdSpy](KmdKit18/examples/advanced/KbdSpy)

This is an example of a simple legacy (non PnP) PS/2-keyboard filter driver.
WARNING: You will fail to attach it to USB-keyboard stack.

Tested under: Windows 2000, XP and Server 2003.

## advanced/MouSpy

[advanced/MouSpy](KmdKit18/examples/advanced/MouSpy)

Four-F, four-f@mail.ruThese macro files is from my package cocomac.

This is an example of a simple legacy (non PnP) PS/2-mouse filter driver.
WARNING: You will fail to attach it to USB-mouse stack.

Tested under: Windows 2000, XP and Server 2003.

## advanced/SecureDevices

[advanced/SecureDevices](KmdKit18/examples/advanced/SecureDevices)

This is an example how to apply particular security settings
to named device object by calling IoCreateDeviceSecure instead of
IoCreateDevice.

Since IoCreateDeviceSecure routine is not a part of the operating system,
we link wdmsec.lib, which contains all needed routines.

The wdmsec.lib library you will find here is not one shipped with the DDK.
I had to rebuild it to reduce its size, removing not needed members.

## basic/FileWorks

[basic/FileWorks](KmdKit18/examples/basic/FileWorks)

This is an example how to create, write to, read from and delete the file.

Use KmdManager to register/unregister and start it.
Watch its debug output with the DbgView (www.sysinternals.com) or use SoftICE.

Tested under: Windows 2000, XP and Server 2003.

## basic/HiddenDriver

[basic/HiddenDriver](KmdKit18/examples/basic/HiddenDriver)

This is an example how to pack the driver into resources.

Tested under: Windows 2000, XP and Server 2003.

## basic/IsSafeBootMode

[basic/IsSafeBootMode](KmdKit18/examples/basic/IsSafeBootMode)

The way you know whether the system is running in safe mode or not.

Use KmdManager to register/unregister and start it.
Watch its debug output with the DbgView (www.sysinternals.com) or use SoftICE.

Tested under: Windows 2000, XP and Server 2003.

## basic/MemoryWorks/LookasideList

[basic/MemoryWorks/LookasideList](KmdKit18/examples/basic/MemoryWorks/LookasideList)

This is an example how to use lookaside list and doubly linked list
to manage the memory blocks allocated from lookaside list.

Use KmdManager to register/unregister and start/stop it.
Watch its debug output with the DbgView (www.sysinternals.com) or use SoftICE.

Tested under: Windows 2000, XP and Server 2003.

## basic/MemoryWorks/seh

[basic/MemoryWorks/seh](KmdKit18/examples/basic/MemoryWorks/seh)

This is an example how to handle exceptions with SEH. But remember.
YOU CAN'T HANDLE ALL EXCEPTIONS WITH SEH IN KERNEL MODE !

Use KmdManager to register/unregister and start/stop it.
Watch its debug output with the DbgView (www.sysinternals.com) or use SoftICE.

Tested under: Windows 2000, XP and Server 2003.

## basic/MemoryWorks/SharedSection

[basic/MemoryWorks/SharedSection](KmdKit18/examples/basic/MemoryWorks/SharedSection)

This is an example how to use named section object to share memory
between user mode process and kernel mode driver.

Tested under: Windows 2000, XP and Server 2003.

## basic/MemoryWorks/SharingMemory

[basic/MemoryWorks/SharingMemory](KmdKit18/examples/basic/MemoryWorks/SharingMemory)

This is an example of one possible way to share
the memory buffer between user and kernel mode.

Tested under: Windows 2000, XP and Server 2003.

## basic/MemoryWorks/SystemModules

[basic/MemoryWorks/SystemModules](KmdKit18/examples/basic/MemoryWorks/SystemModules)

This is an example how to allocate memory in kernel mode.
To not allocate it useless we fill it with some system info.

Use KmdManager to register/unregister and start/stop it.
Watch its debug output with the DbgView (www.sysinternals.com) or use SoftICE.

Tested under: Windows 2000, XP and Server 2003.

## basic/RegistryWorks

[basic/RegistryWorks](KmdKit18/examples/basic/RegistryWorks)

This is an example how to create, set, read and delete the registry key.
Use KmdManager to register/unregister and start it.
Watch its debug output with the DbgView (www.sysinternals.com) or use SoftICE.

Tested under: Windows 2000, XP and Server 2003.

## basic/Sections

[basic/Sections](KmdKit18/examples/basic/Sections)

This is an example how to put code in different PE-sections.

## basic/Synchronization/MutualExclusion

[basic/Synchronization/MutualExclusion](KmdKit18/examples/basic/Synchronization/MutualExclusion)

This is an example how to implement mutual exclusive access to the resource.

Use KmdManager to register/unregister and start/stop it.
Watch its debug output with the DbgView (www.sysinternals.com) or use SoftICE.

## basic/Synchronization/SharedEvent - ProcessMon

[basic/Synchronization/SharedEvent - ProcessMon](KmdKit18/examples/basic/Synchronization/SharedEvent - ProcessMon)

This is an example how to notify the user mode about some sort of event has happened.

Tested under: Windows 2000, XP and Server 2003.

## basic/Synchronization/TimerWorks

[basic/Synchronization/TimerWorks](KmdKit18/examples/basic/Synchronization/TimerWorks)

This is an example how to create, set, wait for and cancel the timer.
Use KmdManager to register/unregister and start/stop it.
Watch its debug output with the DbgView (www.sysinternals.com) or use SoftICE.

Tested under: Windows 2000, XP and Server 2003.

## basic/WorkItem

[basic/WorkItem](KmdKit18/examples/basic/WorkItem)

WorkItem Example

## nodriver/DiskGeometry

[nodriver/DiskGeometry](KmdKit18/examples/nodriver/DiskGeometry)

In this example we deal with existing disk device.

Tested on: Windows 2000, XP & Server 2003

## nodriver/DriveLayout

[nodriver/DriveLayout](KmdKit18/examples/nodriver/DriveLayout)

Provides information about features of each disk partition.

Tested on: Windows 2000, XP & Server 2003

## nodriver/FloppyGeometry

[nodriver/FloppyGeometry](KmdKit18/examples/nodriver/FloppyGeometry)

In this example we deal with existing disk device.

Tested on: Windows 2000, XP & Server 2003

## nodriver/KbdGarland

[nodriver/KbdGarland](KmdKit18/examples/nodriver/KbdGarland)

In this example we deal with existing keyboard device.

Tested on: Windows 2000, XP & Server 2003

## nodriver/KbdTypematic

[nodriver/KbdTypematic](KmdKit18/examples/nodriver/KbdTypematic)

In this example we deal with existing keyboard device.
This example offers some of standard Control Pannel -> Keyboard applet functionality.

Tested on: Windows 2000, XP & Server 2003

## nodriver/MbrDump

[nodriver/MbrDump](KmdKit18/examples/nodriver/MbrDump)

Reading MBR from user-mode is easiest thing to do
if you know what device to read from.

Tested on: Windows 2000, XP & Server 2003

## nodriver/SerialBaudRate

[nodriver/SerialBaudRate](KmdKit18/examples/nodriver/SerialBaudRate)

In this example we deal with existing serial device.

Tested on: Windows 2000, XP & Server 2003

## setup/EnumDisk

[setup/EnumDisk](KmdKit18/examples/setup/EnumDisk)

This is an example how to use some of the setup device functions.
Enumerates all available disk devices and gets the device property.
Run it from command line.

Tested on: Windows 2000, XP & Server 2003

## simple/Beeper

[simple/Beeper](KmdKit18/examples/simple/Beeper)

How to beep with system speaker.

Tested on: Windows NT4.0+sp6, 2000, XP & Server 2003

## simple/DateTime

[simple/DateTime](KmdKit18/examples/simple/DateTime)

Gives direct port I/O access to a user mode process
Based on Dale Roberts' article and c-source code.

Tested on: Windows 2000, XP & Server 2003

## simple/GetKernelBase

[simple/GetKernelBase](KmdKit18/examples/simple/GetKernelBase)

This is an example how to find the base of ntoskrnl.exe.

Use KmdManager to register/unregister and start/stop it.
Watch its debug output with the DbgView (www.sysinternals.com) or use SoftICE.

Tested on: Windows 2000, XP & Server 2003

## simple/NtBuild

[simple/NtBuild](KmdKit18/examples/simple/NtBuild)

Demonstrates driver communication with ReadFile,
using neither method. And how to use SEH in ring0.

Tested on: Windows 2000, XP & Server 2003

## simple/Simplest

[simple/Simplest](KmdKit18/examples/simple/Simplest)

Simplest possible Kernel-Mode Driver that does absolutely nothing.

Tested on: Windows 2000, XP & Server 2003

## simple/Skeleton

[simple/Skeleton](KmdKit18/examples/simple/Skeleton)

The skeleton code for a Windows NT Kernel-Mode Device Driver.

Tested on: Windows 2000, XP & Server 2003

## simple/VirtToPhys

[simple/VirtToPhys](KmdKit18/examples/simple/VirtToPhys)

Demonstrates driver communication with DeviceIoControl,
using buffered method. And how to translate virtual addresses.

Tested on: Windows 2000, XP & Server 2003

## simple/WhichIrqlAndContext

[simple/WhichIrqlAndContext](KmdKit18/examples/simple/WhichIrqlAndContext)

This example let you know the IRQL and Process/Thread context
at which the main driver's routines are running.
Use DebugView (www.sysinternals.com) to watch its output.

Tested on: Windows 2000, XP & Server 2003

## Related

- https://masm32.com/board/index.php?topic=4452.0
- http://www.masm32.com/website/kmdtute/index.html
- https://www.asmcommunity.net/forums/topic/10872/2.html
- http://four-f.narod.ru/
- https://empyreal96.github.io/nt-info-depot/WinDDK.html

# More info to learn

- https://github.com/therealdreg/x86osdev

- Windows Driver Kit (WDK) Version 7.1.0 docs: https://www.microsoft.com/en-us/download/details.aspx?id=11800

- WDK docs: https://learn.microsoft.com/en-us/windows-hardware/drivers/download-the-wdk 

- Intel 64 and IA-32 Architectures Software Developer's Manual Combined Volumes: 1, 2A, 2B, 2C, 2D, 3A, 3B, 3C, 3D, and 4

- Windows 2003 source code leak, WRK, CRK ...

- Undocumented Windows NT book by Prasad Dabak, Sandeep Phadke & Milind Borate

- Windows Kernel Programming book by Pavel Yosifovich

- Windows Internals books by Mark Russinovich, David A. Solomon & others

- (book) The Rootkit Arsenal 2nd by Bill Blunden

- (book) What Makes It Page?: The Windows 7 (x64) Virtual Memory Manager by Enrico Martignetti

- (book) Subverting the Windows Kernel

- (book) Rootkits and Bootkits: Reversing Modern Malware and Next Generation Threats

- osdev (community for those people interested in OS development)

- random github/gist, OSR, phrack, uninformed, openrce, kernelmode, rootkit.com, arteam...

# People to follow

- https://github.com/hfiref0x
- https://github.com/rwfpl
- https://github.com/deroko
- https://github.com/Fyyre
- https://github.com/Cr4sh
- https://twitter.com/zwclose
- https://twitter.com/yarden_shafir
- https://twitter.com/markrussinovich
- https://twitter.com/aionescu
- https://twitter.com/zodiacon
- https://github.com/TheEnergyStory
- https://twitter.com/msuiche
- https://twitter.com/therealdreg
- https://twitter.com/0vercl0k
- https://twitter.com/Ivanlef0u
- https://twitter.com/mrexodia
- https://twitter.com/reversemode
- https://twitter.com/standa_t
- https://twitter.com/richinseattle
- https://twitter.com/gynvael
- https://twitter.com/j00ru
- https://twitter.com/Xylit0l
