#*******************************************************************************
# Makefile for Cross-Compile to ARM target. Tested with RaspberryPi v2
# Dung Le, 2012
#
# Usage:
# - change "NAME" below with your assembly program
# - build (assembling and linking):
#   > make
# - debug:
#   > make db
# note: kernel.img can be preloaded to SDcard
#
#*******************************************************************************

NAME = i2c

CROSS_COMPILE ?= arm-elf
CC := $(CROSS_COMPILE)-gcc
LD := $(CROSS_COMPILE)-ld
AS := $(CROSS_COMPILE)-as
OBJCOPY := $(CROSS_COMPILE)-objcopy
OBJDUMP := $(CROSS_COMPILE)-objdump

DB   = gdb-multiarch
AFLAGS  = -gstabs+ -warn -fatal-warnings
LFLAGS  = -T memmap

OBJS = $(NAME).o delay.o led.o

all: $(NAME).elf kernel.img

kernel.img:	$(NAME)
	$(OBJCOPY) -O binary $< $@
	
$(NAME).elf: $(NAME)
	$(OBJCOPY) $< $@
	$(OBJDUMP) -D $@ > $(NAME).lst

$(NAME): $(OBJS)	
	$(LD) $^ -o $@ $(LFLAGS) --M $@.mp
	

%.o: %.s
	$(AS) $< -o $@ $(AFLAGS)

db: $(NAME).elf
	$(DB) -x gdbscript $<
	
clean:
	rm -f *.lst *.elf *.mp *.o $(NAME) kernel.img
