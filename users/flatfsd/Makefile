######################################################

VERSION = 3.0.0

######################################################
ifeq ($(wildcard $(DIR_USERS)/.config),)
include ../.config
else
include $(DIR_USERS)/.config
endif


EXEC = flatfsd
OBJS = flatfsd.o newfs.o reboot.o

MTDAPI_DIR=$(DIR_ROOT)/users/lib/lib

# force error checking on compile
#CFLAGS += -Wall -Werror
CFLAGS += -Wall

# CFLAGS += -DDEBUG

######################################################
#
# Erk! settings to change the way flatfsd works
#CONFIG_USER_FLATFSD_XXX=y
#CONFIG_USER_FLATFSD_AUTO=y
# Explicit setting overrides autodetect
#USING_FLASH_FILESYSTEM_y = y
#USING_DISKLIKE_DEVICE_$(CONFIG_USER_FLATFSD_DISKLIKE) = y
##ifdef CONFIG_USER_FLATFSD_AUTO
##	ifdef CONFIG_NFTL
##		ifndef CONFIG_MTD_CFI_INTELEXT
##			USING_DISKLIKE_DEVICE_$(CONFIG_SH_SECUREEDGE5410) = y
##		endif
##	endif
##	ifeq ($(USING_DISKLIKE_DEVICE_y),)
##		USING_MTD_DEVICE_$(CONFIG_MTD) = y
##		USING_BLKMEM_DEVICE_$(CONFIG_BLK_DEV_BLKMEM) = y
##	endif
##endif

##ifdef CONFIG_USER_FLATFSD_COMPRESSED
##	OBJS += flatfs3.o
##	EXTRA_LIBS = -lz
##endif

USING_MTD_DEVICE_y=1
##USING_BLKMEM_DEVICE_y=1
######################################################

ifdef USING_DISKLIKE_DEVICE_y
CFLAGS += -DUSING_DISKLIKE_DEVICE=1
OBJS += flatfs.o flatfs1.o disk.o ops.o
SUBSYS = disk
else
ifdef USING_MTD_DEVICE_y
CFLAGS += -DUSING_MTD_DEVICE=1
OBJS += flatfs.o flatfs1.o mtd.o ops.o
SUBSYS = mtd
else
ifdef USING_BLKMEM_DEVICE_y
CFLAGS += -DUSING_BLKMEM_DEVICE=1
OBJS += flatfs.o flatfs1.o blkmem.o ops.o
SUBSYS = blkmem
else
ifdef USING_FLASH_FILESYSTEM_y
CFLAGS += -DUSING_FLASH_FILESYSTEM=1
SUBSYS = fs
else
all:: ERROR-no-flatfsd-storage-type-defined
endif
endif
endif
endif

ifdef CONFIG_PROP_LOGD_LOGD
CFLAGS += -DLOGGING
endif
ifdef CONFIG_USER_FLATFSD_HAS_RTC
CFLAGS += -DHAS_RTC
endif

CFLAGS += -DFLATFSD_VERSION='"$(VERSION)$(SUBSYS)"'

ifeq ($(CONFIG_MTD_NAME_MAPPING),y)
CFLAGS += -DMTD_NAME_MAPPING
LDFLAGS += -L$(MTDAPI_DIR) -lmtdapi
endif
######################################################

all:: $(EXEC)

$(EXEC): $(OBJS)
	$(CC) $(LDFLAGS) -o $@ $(OBJS) $(EXTRA_LIBS) $(LDLIBS)

romfs:
	$(ROMFSINST) /bin/$(EXEC)
ifeq ($(CONFIG_MTD_NAME_MAPPING),y)
	$(ROMFSINST) $(DIR_ROOT)/users/lib/lib/libmtdapi.so /lib/libmtdapi.so
endif

clean:
	-rm -f $(EXEC) *.elf *.gdb *.o flatfsd

######################################################

$(OBJS): Makefile flatfs.h

######################################################
