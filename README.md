# Linux on CologneChip Gatemate

Install https://www.scala-sbt.org/1.x/docs/Installing-sbt-on-Linux.html#Installing+sbt+on+Linux pour installer sbt (see section Debian)

Install the Gatemate synthesis tools: wget https://github.com/YosysHQ/oss-cad-suite-build/releases/ and
```
export PATH=$PATH:$HOME/oss-cad-suite/bin
```

Add https://github.com/litex-hub/linux-on-litex-vexriscv to the litex install 

```
cd linux-on-litex-vexriscv
./make.py --board=colognechip_gatemate_evb --cpu-count=1 --rootfs mmcblk0p2 --build
openFPGALoader --board gatemate_evb_jtag --index-chain 0 --bitstream build/colognechip_gatemate_evb/gateware/colognechip_gatemate_evb.bit
```

Serial port on top pins of PMODB (``/dev/ttyUSB2``) for the RISC-V console

SD card on PMODA

## Buildroot:

```
export BR2_EXTERNAL=$HOME/linux-on-litex-vexriscv/buildroot
make litex_vexriscv_defconfig
```

Smaller Linux kernel size: disable ``Load all symbols for debugging/ksymoops`` in
```
make linux-menuconfig
General Setup -> Load all symbols for debugging/ksymoops
```
and disable all features of ``Kernel hacking`` as well as SMP support (single CPU).

Edit on the first partition of the SD card image and edit ``boot.json`` to
shrink addresses below the 8 MB limit. Since ``Image`` is 7283096 bytes:

```
{
        "Image"       : "0x40000000",
        "rv32.dtb"    : "0x406f6000",
        "opensbi.bin" : "0x40700000"
}
```

leads to
```
        __   _ __      _  __     
       / /  (_) /____ | |/_/     
      / /__/ / __/ -_)>  <       
     /____/_/\__/\__/_/|_|       
   Build your hardware, easily!  
      
 (c) Copyright 2012-2026 Enjoy-Digital
 (c) Copyright 2007-2015 M-Labs  
      
 BIOS CRC passed (5a284fa9)      
      
 LiteX git sha1: afed66238       
      
--================ SoC =================--     
CPU:            VexRiscv SMP-LINUX @ 24MHz     
BUS:            wishbone 32-bit data/32-bit addr        
CSR:            32-bit data big ordering       
ROM:            64.0KiB 
SRAM:           6.0KiB  
FLASH:          8.0MiB  
MAIN RAM:       8.0MiB  
      
HyperRAM init...        
HyperRAM Clk Ratio 4:1  
HyperRAM Variable Latency: 3 CK (X1)  
HyperRAM Drive Strength: 19 ohm  
HyperRAM Configuration Register 0: ffe7        
      
--=========== Initialization ===========--     
Memtest at 0x40000000 (2.0MiB)...
  Write: 0x40000000-0x40200000 2.0MiB 
   Read: 0x40000000-0x40200000 2.0MiB 
Memtest OK     
Memspeed at 0x40000000 (Sequential, 2.0MiB)... 
  Write speed: 11.3MiB/s
   Read speed: 7.0MiB/s 
      
Initializing mx25r6435f SPI Flash @0x00800000...        
SPI Flash clk configured to 12 MHz (div: 2)    
Memspeed at 0x800000 (Sequential, 4.0KiB)...   
   Read speed: 980.7KiB/s        
Memspeed at 0x800000 (Random, 4.0KiB)...       
   Read speed: 397.7KiB/s        
      
--================ Boot ================--     
Booting from serial...  
Press Q or ESC to abort boot completely.       
sL5DdSMmkekro  
             Timeout    
Booting from SDCard in SD-Mode...  
Booting from boot.json...
Copying Image to 0x40000000 (7283096 bytes)...      
[########################################] 
Copying rv32.dtb to 0x406f6000 (3213 bytes)...      
[########################################] 
Copying opensbi.bin to 0x40700000 (263636 bytes)... 
[########################################] 
Executing booted program at 0x40700000     
                         
--============== Liftoff! ==============-- 
                         
OpenSBI v1.3             
   ____                    _____ ____ _____
  / __ \                  / ____|  _ \_   _|        
 | |  | |_ __   ___ _ __ | (___ | |_) || | 
 | |  | | '_ \ / _ \ '_ \ \___ \|  _ < | | 
 | |__| | |_) |  __/ | | |____) | |_) || |_
  \____/| .__/ \___|_| |_|_____/|____/_____|        
        | |              
        |_|              
                         
Platform Name             : LiteX / VexRiscv-SMP    
Platform Features         : medeleg        
Platform HART Count       : 8     
Platform IPI Device       : aclint-mswi    
Platform Timer Device     : aclint-mtimer @ 100000000Hz  
Platform Console Device   : litex_uart     
Platform HSM Device       : ---   
Platform PMU Device       : ---   
Platform Reboot Device    : ---   
Platform Shutdown Device  : ---   
Platform Suspend Device   : ---   
Platform CPPC Device      : ---   
Firmware Base             : 0x40700000     
Firmware Size             : 376 KB
Firmware RW Offset        : 0x40000        
Firmware RW Size          : 120 KB
Firmware Heap Offset      : 0x52000        
Firmware Heap Size        : 48 KB (total), 3 KB (reserved), 8 KB (used), 36 KB (free)    
Firmware Scratch Size     : 4096 B (total), 452 B (used), 3644 B (free)    
Runtime SBI Version       : 1.0   
                         
Domain0 Name              : root  
Domain0 Boot HART         : 0     
Domain0 HARTs             : 0*,1*,2*,3*,4*,5*,6*,7* 
Domain0 Region00          : 0xf0018000-0xf001bfff M: (I,R,W) S/U: ()       
Domain0 Region01          : 0xf0010000-0xf0017fff M: (I,R,W) S/U: ()       
Domain0 Region02          : 0x40740000-0x4075ffff M: (R,W) S/U: ()
Domain0 Region03          : 0x40700000-0x4073ffff M: (R,X) S/U: ()
Domain0 Region04          : 0x00000000-0xffffffff M: (R,W,X) S/U: (R,W,X)  
Domain0 Next Address      : 0x40000000     
Domain0 Next Arg1         : 0x40ef0000     
Domain0 Next Mode         : S-mode
Domain0 SysReset          : yes   
Domain0 SysSuspend        : yes   
                                  
Boot HART ID              : 0     
Boot HART Domain          : root  
Boot HART Priv Version    : unknown        
Boot HART Base ISA        : rv32ima        
Boot HART ISA Extensions  : zicntr
Boot HART PMP Count       : 0     
Boot HART PMP Granularity : 0     
Boot HART PMP Address Bits: 0     
Boot HART MHPM Count      : 0     
Boot HART MIDELEG         : 0x00000222     
Boot HART MEDELEG         : 0x0000b109     
```
but fails to load and execute the Linux kernel
