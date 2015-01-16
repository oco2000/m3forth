spf=../../spf4

all: *.spf
	$(spf) testasm.spf
	arm-none-eabi-objdump -m arm --disassembler-options=force-thumb -b binary -EL -w -D testasm.bin > testasm.lst
