
compiler = ../../spf4 src/options.spf
# project name
project = led
# device used
device = devices/stm32vldiscovery.spf
# where is st-link
st_path = ~/src/stlink/build/Release/bin/
openocd = openocd
gdb = gdb-multiarch

.PHONY: proteus
# make project
$(project).spf.elf: *.spf
	$(compiler) -h -i -O -D S\" $(device)\" S\" $(project).spf\" m3forth.spf
	arm-none-eabi-objdump -S -w $(project).spf.elf > $(project).spf.lst
	readelf -Ww $(project).spf.elf > $(project).spf.debug
	readelf -aW $(project).spf.elf > $(project).spf.info
	chmod o+x $(project).spf.elf

# debug project with qemu, uart emulation or semihosting and ddd
debug: $(project).spf.elf
	killall qemu-system-arm ; sleep 1 && qemu-system-arm -M lm3s811evb -cpu cortex-m3 -s -S -nographic -monitor null -serial telnet:localhost:1235,server -semihosting -kernel $(project).spf.elf &
	sleep 1 && xfce4-terminal -x rlwrap nc localhost 1235 &
	sleep 1 && ddd --eval-command="target remote localhost:1234" --debugger $(gdb) $(project).spf.elf ; killall qemu-system-arm

# make for proteus
proteus: *.spf
	$(compiler) -b -h -i -O -D S\" devices/proteus_device.spf\" S\" $(project).spf\" m3forth.spf
	arm-none-eabi-objdump -S -w $(project).spf.elf > $(project).spf.lst
#	readelf -Ww $(project).spf.elf > $(project).spf.debug
#	readelf -aW $(project).spf.elf > $(project).spf.info
	chmod o+x $(project).spf.elf

# remove unnesessary files
clean:
	rm $(project).spf.lst $(project).spf.bin $(project).spf.elf $(project).spf.debug $(project).spf.info

# program project with st-link
flash: $(project).spf.elf
	sudo $(st_path)st-flash write $(project).spf.bin 0x8000000

# debug project with st-link and ddd
debug2: $(project).spf.elf
	sudo $(st_path)st-util &
	sleep 1 && ddd --eval-command="target extended-remote localhost:4242" --debugger $(gdb) $(project).spf.elf ; sudo killall st-util

# debug project with openocd and ddd
debug3: $(project).spf.elf
	xfce4-terminal -e 'sudo rlwrap $(openocd) -f stm32vldiscovery.cfg' &
	sleep 10 && ddd --eval-command="target extended-remote localhost:4242" --debugger $(gdb) $(project).spf.elf
