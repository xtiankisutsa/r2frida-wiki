# r2frida-wiki

Before reading this tutorial, it's highly recommended that you first take a look at the official website of [r2frida](https://github.com/nowsecure/r2frida) to install the tool as well as understand the capabilities of this one.

Commands
=========
In order to get the list of commands, you might want to type: `\?`

## Informative commands (`\i`)
```java
[0x00000000]> \?~^i
i                          Show target information
ii[*]                      List imports
il                         List libraries
is[*] <lib>                List symbols of lib (local and global ones)
iE[*] <lib>                Same as is, but only for the export global ones
iEa[*] (<lib>) <sym>       Show address of export symbol
isa[*] (<lib>) <sym>       Show address of symbol
ic <class>                 List Objective-C classes or methods of <class>
ip <protocol>              List Objective-C protocols or methods of <protocol>
```
- `\i`: Shows targer information
```java
[0x00000000]> \i
arch  arm
bits  64
os  linux
pid  14473
uid  10127
objc  false
java  true
cylang  false
```
- `\i*`: Shows target information in r2 form
```java
[0x00000000]> \i*
e asm.arch=arm
e asm.bits=64
e asm.os=linux
```
- `\il`
- `\iE (lib)`: List exports of library(ies)
```java
[0xd0d77878]> \iE* frida-agent-32.so
f sym.fun.frida_agent_main = 0xd671a2cd
f sym.var.FRIDA_AGENT_1.0 = 0x0
```

- `\ii (lib)`: List imports of library(ies)
```java
[0xd0d77878]> \ii frida-agent-32.so~open
0xeeb94eb9 f opendir /system/lib/libc.so
0xeebc9eb5 f freopen /system/lib/libc.so
0xeebc9ce9 f fopen /system/lib/libc.so
0xeebc9e09 f fdopen /system/lib/libc.so
0xeeb97b15 f open /system/lib/libc.so
0xeeb94e31 f fdopendir /system/lib/libc.so
0xeeb97ba1 f openat /system/lib/libc.so
0xef39ac81 f dlopen /system/bin/linker
```

## Search commands (`\/`)
```java
[0x00000000]> \?~^/
/[x][j] <string|hexpairs>  Search hex/string pattern in memory ranges (see search.in=?)
/w[j] string               Search wide string
/v[1248][j] value          Search for a value honoring `e cfg.bigendian` of given width
```

## Dynamic/Debugging commands (`\d`)
```java
[0x00000000]> \?~^d
db (<addr>|<sym>)          List or place breakpoint
db- (<addr>|<sym>)|*       Remove breakpoint(s)
dc                         Continue breakpoints or resume a spawned process
dd[-][fd] ([newfd])        List, dup2 or close filedescriptors
dm[.|j|*]                  Show memory regions
dma <size>                 Allocate <size> bytes on the heap, address is returned
dmas <string>              Allocate a string inited with <string> on the heap
dmad <addr> <size>         Allocate <size> bytes on the heap, copy contents from <addr>
dmal                       List live heap allocations created with dma[s]
dma- (<addr>...)           Kill the allocations at <addr> (or all of them without param)
dmp <addr> <size> <perms>  Change page at <address> with <size>, protection <perms> (rwx)
dmm                        List all named squashed maps
dmh                        List all heap allocated chunks
dmhj                       List all heap allocated chunks in JSON
dmh*                       Export heap chunks and regions as r2 flags
dmhm                       Show which maps are used to allocate heap chunks
dp                         Show current pid
dpt                        Show threads
dr                         Show thread registers (see dpt)
dl libname                 Dlopen a library
dl2 libname [main]         Inject library using Frida's >= 8.2 new API
dt (<addr>|<sym>) ...      Trace list of addresses or symbols
dth (<addr>|<sym>) (x y..) Define function header (z=str,i=int,v=hex barray,s=barray)
dt-                        Clear all tracing
dtr <addr> (<regs>...)     Trace register values
dtf <addr> [fmt]           Trace address with format (^ixzO) (see dtf?)
dtSf[*j] [sym|addr]        Trace address or symbol using the stalker (Frida >= 10.3.13)
dtS[*j] seconds            Trace all threads for given seconds using the stalker
di[0,1,-1] [addr]          Intercept and replace return value of address
dx [hexpairs]              Inject code and execute it (TODO)
dxc [sym|addr] [args..]    Call the target symbol with given args
```

- `\dt (<addr>|<sym>) ...`: Trace list of addresses or symbols. Similar to `frida-trace`
```java
[0x00000000]> \dt fopen; \dth fopen z; \dc
[TRACE] 0x7f942bf1e8 ( fopen ) ["/proc/self/maps"]
 - 0x7f5bf72d98 libtarget.so!scan_executable+0xf0
 - 0x7f5bf72d94 libtarget.so!scan_executable+0xec
 - 0x7f5bf721b4 libtarget.so!secchecks+0x90
 - 0x7f5bf6cdb8 libtarget.so!Java_com_super_secure_App+0x1710
 - 0x7f69e6a7e0 base.odex!oatexec+0x4e7e0
```

Android
=======

Attaching
---------

Spawning
--------

iOS
===

**TODO**
