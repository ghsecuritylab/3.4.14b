ifeq ($(wildcard $(DIR_USERS)/.config),)
include ../.config
else
include $(DIR_USERS)/.config
endif
KLINK_DIR=$(shell pwd)
COMMON_CFLAGS =

ifeq ($(CONFIG_APP_MULTI_AP),y)
COMMON_CFLAGS += -DRTK_MULTI_AP
COMMON_CFLAGS += -DUNIVERSAL_REPEATER
COMMON_CFLAGS += -DWIFI_SIMPLE_CONFIG
endif
COMMON_CFLAGS += -DCONFIG_RTL_8197F

CC = msdk-linux-gcc

APMIB_DIR = ../boa/apmib

CFLAGS := -s -Wall -Os
LDFLAGS := -s -Wall
IFLAGS := -I. -I$(APMIB_DIR)

TARGET = klinkM klinkS klinkLinksListFunc.a

all: $(TARGET)


klinkM: klinkMaster.o cJSON.o klinkLinksListFunc.o
	$(CC) klinkMaster.o cJSON.o klinkLinksListFunc.o -lm $(LDFLAGS) $(COMMON_CFLAGS) $(IFLAGS) -L$(APMIB_DIR) -lapmib  -o klinkM -lrt 

klinkS: klinkSlave.o cJSON.o version.o
	$(CC) klinkSlave.o cJSON.o version.o -lm $(LDFLAGS) $(COMMON_CFLAGS)  $(IFLAGS) -L$(APMIB_DIR) -lapmib  -o klinkS -lrt 

cJSON.o:cJSON.c
	$(CC) -c cJSON.c -o cJSON.o -lrt -lm
 
klinkSlave.o:klinkSlave.c 
	$(CC) -c klinkSlave.c  $(IFLAGS) -o klinkSlave.o

klinkMaster.o:klinkMaster.c
	$(CC) -c klinkMaster.c  $(IFLAGS) -o klinkMaster.o
klinkLinksListFunc.o:klinkLinksListFunc.c
	$(CC) -c klinkLinksListFunc.c  $(IFLAGS) -o klinkLinksListFunc.o
version.o: 	../boa/src/version.c
	$(CC) $(CFLAGS) $(IFLAGS) $(COMMON_CFLAGS) -c -o $@ $<
## libEntrydExt.a
LIBENTRYDEXT=klinkLinksListFunc.a
$(LIBENTRYDEXT): klinkLinksListFunc.o klink.h 
	@echo "[AR] $@"
	@$(AR) -r $@ klinkLinksListFunc.o
	
romfs:
	$(ROMFSINST) $(KLINK_DIR)/klinkM /bin/
	$(ROMFSINST) $(KLINK_DIR)/klinkS /bin/
	
.PHONY: clean

clean:
	rm -f *o
	rm -f $(TARGET)
