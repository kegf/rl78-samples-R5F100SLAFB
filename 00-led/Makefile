PROJECT_ELF=led.elf
PROJECT_MOT=$(PROJECT_ELF:.elf=.mot)
PROJECT_MAP=$(PROJECT_ELF:.elf=.map)
PROJECT_LST=$(PROJECT_ELF:.elf=.lst)

PREFIX:=rl78-elf

LD = $(PREFIX)-gcc
CC = $(PREFIX)-gcc
AS = $(PREFIX)-gcc
OBJCOPY = $(PREFIX)-objcopy
OBJDUMP = $(PREFIX)-objdump
SIZE = $(PREFIX)-size

COMMON_PATH = ../common
PROJECT_PATH = .

PROJECT_LNK = $(COMMON_PATH)/rl78-R5F100SL.ld

CFLAGS = -Os -ggdb -ffunction-sections -fdata-sections -I$(PROJECT_PATH) -I$(COMMON_PATH)
LDFLAGS = -Wl,--gc-sections -Wl,-Map=$(PROJECT_MAP) -T $(PROJECT_LNK) -nostartfiles

SOURCES = $(PROJECT_PATH)/main.c \
	$(END)

OBJS	= $(SOURCES:.c=.o) $(COMMON_PATH)/crt0.o

.PHONY: all

all: $(PROJECT_MOT) $(PROJECT_LST)
	$(SIZE) $(PROJECT_ELF)

rom: $(PROJECT_MOT)

$(PROJECT_MOT): $(PROJECT_ELF)
	$(OBJCOPY) -O srec $^ $@

$(PROJECT_LST): $(PROJECT_ELF)
	$(OBJDUMP) -DS $^ > $@

$(PROJECT_ELF): $(OBJS)
	$(LD) $(LDFLAGS) -o $@ $^

clean:
	-rm -f $(OBJS) $(PROJECT_ELF) $(PROJECT_MOT) $(PROJECT_MAP) $(PROJECT_LST)
