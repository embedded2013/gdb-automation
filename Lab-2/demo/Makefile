CROSS_COMPILE=arm-none-eabi-
QEMU_STM32 ?= ../qemu_stm32/arm-softmmu/qemu-system-arm

ARCH=CM3
VENDOR=ST
PLAT=STM32F10x
CMSIS_LIB=libraries/CMSIS/$(ARCH)
STM32_LIB=libraries/STM32F10x_StdPeriph_Driver

all: main.bin

main.bin: main.c
	$(CROSS_COMPILE)gcc \
		-Wl,-Tmain.ld -nostartfiles \
		-I. -Ilibraries/CMSIS/CM3/CoreSupport \
		-Ilibraries/CMSIS/CM3/DeviceSupport/ST/STM32F10x \
		-Ilibraries/STM32F10x_StdPeriph_Driver/inc \
		-fno-common -O0 \
		-gdwarf-2 -g3 \
		-mcpu=cortex-m3 -mthumb \
		-o main.elf \
		\
$(CMSIS_LIB)/CoreSupport/core_cm3.c \
$(CMSIS_LIB)/DeviceSupport/$(VENDOR)/$(PLAT)/system_stm32f10x.c \
$(CMSIS_LIB)/DeviceSupport/$(VENDOR)/$(PLAT)/startup/gcc_ride7/startup_stm32f10x_md.s \
$(STM32_LIB)/src/stm32f10x_rcc.c \
$(STM32_LIB)/src/stm32f10x_gpio.c \
$(STM32_LIB)/src/stm32f10x_usart.c \
\
		main.c
	$(CROSS_COMPILE)objcopy -Obinary main.elf main.bin
	$(CROSS_COMPILE)objdump -S main.elf > main.list

qemu: main.bin
	$(QEMU_STM32) -M stm32-p103 -kernel main.bin

qemudbg: main.bin
	$(QEMU_STM32) -M stm32-p103 \
		-gdb tcp::3333 -S \
		-kernel main.bin

emu: main.bin
	bash emulate.sh main.bin

clean:
	rm -f *.elf *.bin *.list
