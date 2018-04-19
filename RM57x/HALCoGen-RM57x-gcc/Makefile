################################################################################
# Makefile for Hercules TMS570LC43
#
# Author: Ivan Pavic (@dumpram)
################################################################################


PREFIX = arm-none-eabi-
CC = $(PREFIX)gcc
LD = $(PREFIX)ld
OBJCOPY = $(PREFIX)objcopy

CFLAGS   = -Wall -I include
CFLAGS  += -mfpu=vfpv3-d16 -march=armv7-r -mcpu=cortex-r4f
CFLAGS  += -fomit-frame-pointer -fno-strict-aliasing -mlittle-endian
CFLAGS += -I../../example/hdk/inc -I../../lwip-1.4.1/apps/httpserver_raw/
CFLAGS += -I../../lwip-1.4.1/ports/hdk/include -I../../lwip-1.4.1/ -I../../lwip-1.4.1/src/include
CFLAGS += -I../../lwip-1.4.1/src/include/ipv4/
CFLAGS += -D_RM57Lx_ -DLWIP_HTTPD_STRNSTR_PRIVATE=0

LDFLAGS  = -EL --gc-sections

BINFLAGS = -O binary

SRCS  = $(wildcard source/*.c)
SRCS  += $(wildcard ../../example/hdk/src/*.c)
SRCS += $(wildcard ../../lwip-1.4.1/apps/httpserver_raw/fs.c)
SRCS += $(wildcard ../../lwip-1.4.1/apps/httpserver_raw/httpd.c)
SRCS += $(wildcard ../../lwip-1.4.1/src/*.c)
SRCS += $(wildcard ../../lwip-1.4.1/src/core/def.c)
SRCS += $(wildcard ../../lwip-1.4.1/src/core/timers.c)
SRCS += $(wildcard ../../lwip-1.4.1/src/netif/ethernetif.c)
SRCS += $(wildcard ../../lwip-1.4.1/ports/hdk/lwiplib.c)

ASRCS = $(wildcard source/*.s)
OBJS  = $(patsubst %.c, %.o, $(SRCS))
AOBJS = $(patsubst %.s, %.o, $(ASRCS))
LINKER_SCRIPT = $(wildcard source/*.ld)

.SECONDARY:

all: app.bin Makefile

%.bin: %.elf
	$(OBJCOPY) $< $(BINFLAGS) $@

%.elf: $(OBJS) $(AOBJS)
	$(LD) $^ libc.a libm.a libgcc.a $(LDFLAGS) -o $@ -T $(LINKER_SCRIPT) 

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

%.o: %.s
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJS) $(AOBJS) app.elf app.bin
