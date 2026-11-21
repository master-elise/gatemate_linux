# Linux on CologneChip Gatemate

Install https://www.scala-sbt.org/1.x/docs/Installing-sbt-on-Linux.html#Installing+sbt+on+Linux pour installer sbt (see section Debian)

Install the Gatemate synthesis tools: wget https://github.com/YosysHQ/oss-cad-suite-build/releases/download/ and
```
export PATH=$PATH:$HOME/oss-cad-suite/bin
```

Add https://github.com/litex-hub/linux-on-litex-vexriscv to the litex install 

```
cd linux-on-litex-vexriscv
./make.py --board=colognechip_gatemate_evb --cpu-count=1 --build
```

Serial port on top pins of PMODB (/dev/ttyUSB2) for the RISC-V console

SD card on PMODA

## Buildroot:

```
export BR2_EXTERNAL=/home/jmfriedt/WR/linux-on-litex-vexriscv/buildroot
make litex_vexriscv_defconfig
```

Smaller kernel size: disable ``Load all symbols for debugging/ksymoops`` in
```
make linux-menuconfig
General Setup -> Load all symbols for debugging/ksymoops
```
