# INSTALLATION

## ARM binutils
It contains utilities used in the project (arm-none-eabi-objdump). Install **binutils-arm-none-eabi** package.

## ARM debugger
It's not required if you don't want to debug your apps. Install **gdb-multiarch** package.

## qEMU
Get **qEMU** for ARM to debug your programs without a real device. Install **qemu-system-arm** package using your package manager.

## libelf
Get **libelf1** library for elf-files production. In most cases you should just install **libelf1** package.

## libdwarf
Get **libdwarf1** library for debug symbols generation. In most cases you should just install **libdwarf1** package.  For 64-bit Linux install 32-bit package!

## ddd
Get **ddd** (data display debugger). Install **ddd** package.

## rlwrap
It is needed to make work with the forth terminal more convenient. Install **rlwrap** package.

## xfce-terminal
is used to communicate with the target. You can replace it with another terminal emulator, but different command line parameters should be used.

## readelf
To dump elf sections and content. Install **binutils** package.

## netcat
To connect to gdb server. Should be installed in most linux distros. If not - install **nc.openbsd** package.

## ST-link
Get the **stlink** to flash the controller. Install **stlink-tools** package.

## openocd
Get **openocd** if you want to get USB connection to your device via semihosting and program in Forth on you device directly!. install **openocd** package.

## test
To test your installation go to **examples/tester** and run:
```bash
make test
```
