
# vim: ft=make noexpandtab

OBJECTS := mbmp.o

BINDIR_LINUX = ../bin/linux
BINDIR_MSP430G2553 = ../bin/msp430g2553

MD=mkdir -v -p

CFLAGS := -Wall -Werror -I../../mfat/src

$(BINDIR_LINUX)/libmbmp.a: OBJDIR := ../obj/linux

$(BINDIR_MSP430G2553)/libmbmp.a: OBJDIR := ../obj/msp430g2553
$(BINDIR_MSP430G2553)/libmbmp.a: AR := msp430-ar
$(BINDIR_MSP430G2553)/libmbmp.a: CC := msp430-gcc
$(BINDIR_MSP430G2553)/libmbmp.a: CFLAGS += \
	-mmcu=msp430g2553 \
	-g \
	-pg \
	-Os \
	-DUSE_DISK_RO \
	-fno-builtin \
	-ffreestanding \
	-nostdlib

test_mbmp: LDFLAGS += $(shell pkg-config --libs check) -L. -lmbmp
test_mbmp: CFLAGS += -DCHECK -g -Wall -Werror
test_mbmp: OBJDIR := ../obj/test_mbmp

all: $(BINDIR_LINUX)/libmbmp.a $(BINDIR_MSP430G2553)/libmbmp.a

test_mbmp: libmbmp.a check.o
	$(CC) -o $@ $(OBJDIR)/check.o $(LDFLAGS)

$(BINDIR_LINUX)/libmbmp.a: $(addprefix ../obj/linux/,$(OBJECTS))
	$(MD) $(BINDIR_LINUX)
	$(AR) rcs $@ $^

$(BINDIR_MSP430G2553)/libmbmp.a: $(addprefix ../obj/msp430g2553/,$(OBJECTS))
	$(MD) $(BINDIR_MSP430G2553)
	$(AR) rcs $@ $^

../obj/linux/%.o: $(notdir %.c)
	$(MD) $(OBJDIR)
	$(CC) -c -o $@ $< $(CFLAGS)

../obj/msp430g2553/%.o: $(notdir %.c)
	$(MD) $(OBJDIR)
	$(CC) -c -o $@ $< $(CFLAGS)

.PHONY: clean

clean:
	rm -rf ../obj; \
	rm -f test_mbmp; \
	rm -rf ../bin

