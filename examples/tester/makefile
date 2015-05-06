
compiler = ../../spf4
# project name
project = tester
# device used
device = devices/test_device.spf
# where is st-link
st_path = ~/src/stlink

# make project
$(project).spf.elf: *.spf
	$(compiler) options.spf -H -I -O -d -M  S\" $(device)\" S\" $(project).spf\" m3forth.spf
	arm-none-eabi-objdump -S -w $(project).spf.elf > $(project).spf.lst
#	readelf -Ww $(project).spf.elf > $(project).spf.debug
#	readelf -aW $(project).spf.elf > $(project).spf.info
	chmod o+x $(project).spf.elf

# debug project with qemu, uart emulation or semihosting and ddd
debug: $(project).spf.elf
	killall qemu-system-arm ; sleep 1 && qemu-system-arm -M lm3s811evb -cpu cortex-m3 -s -S -nographic -monitor null -serial telnet:localhost:1235,server -semihosting -kernel $(project).spf.elf &
	sleep 1 && xfce4-terminal -x rlwrap nc localhost 1235 &
	sleep 1 && ddd --eval-command="target remote localhost:1234" --debugger arm-none-eabi-gdb $(project).spf.elf ; killall qemu-system-arm

# run project with qemu and uart emulation
run: $(project).spf.elf
	killall qemu-system-arm ; sleep 1 && qemu-system-arm -M lm3s811evb -cpu cortex-m3 -nographic -monitor null -serial telnet:localhost:1235,server -kernel $(project).spf.elf &
	sleep 2 ; rlwrap nc localhost 1235
	killall qemu-system-arm

# run project with qemu with semihosting
run_semi: $(project).spf.elf
	rlwrap qemu-system-arm -M lm3s811evb -cpu cortex-m3 -nographic -monitor null -serial null -semihosting -kernel $(project).spf.elf

# run project with qemu, uart emulation and interpret tester.f,
# full output
test: $(project).spf.elf
	killall qemu-system-arm ; sleep 1 && qemu-system-arm -M lm3s811evb -cpu cortex-m3 -nographic -monitor null -serial telnet:localhost:1235,server -kernel $(project).spf.elf &
	sleep 2 && nc localhost 1235 < tester.f ; killall qemu-system-arm

# run project with qemu, uart emulation and interpret tester.f,
# short output format showing errors only
test2: $(project).spf.elf
	killall qemu-system-arm ; sleep 1 && qemu-system-arm -M lm3s811evb -cpu cortex-m3 -nographic -monitor null -serial telnet:localhost:1235,server -kernel $(project).spf.elf &
	sleep 2 && nc localhost 1235 < tester.f | grep -ai 'incorrect\|wrong\|-?\|completed'; killall qemu-system-arm

# remove unnesessary files
clean:
	rm $(project).spf.lst $(project).spf.bin $(project).spf.elf $(project).spf.debug $(project).spf.info

# program project with st-link
flash: $(project).spf.elf
	sudo $(st_path)/st-flash write v1 $(project).spf.bin 0x8000000

# debug project with st-link and ddd
debug2: $(project).spf.elf
	sudo $(st_path)/st-util &
	sleep 1 && ddd --eval-command="target extended-remote localhost:4242" --debugger arm-none-eabi-gdb $(project).spf.elf ; sudo killall st-util

# debug project with openocd and ddd
debug3: $(project).spf.elf
	xfce4-terminal -e 'sudo rlwrap openocd -f stm32vldiscovery.cfg' &
	sleep 10 && ddd --eval-command="target extended-remote localhost:4242" --debugger arm-none-eabi-gdb $(project).spf.elf

# run with openocd
run3: $(project).spf.elf
	sudo openocd -f stm32vldiscovery.cfg.run

# do test run with openocd
test3: $(project).spf.elf
	sudo openocd -f stm32vldiscovery.cfg.run < tester_short.f | tee test.result
