
obj-y	:= ve890_int.o ve890_api.o 

ifneq "$(CONFIG_RTK_VOIP_DRIVERS_SLIC_ZARLINK_ON_NEW_ARCH)" "y"
obj-y	+= ve890_int_ext.o
endif

#EXTRA_CFLAGS += -Werror
EXTRA_CFLAGS += -DREALTEK_PATCH_FOR_MICROSEMI

ifeq "$(CONFIG_RTK_VOIP_SLIC_ZARLINK_890_SERIES)" "y"
EXTRA_CFLAGS += -I$(DIR_VOIP)/voip_drivers/zarlink/$(ZARLINK_LIB)/vp890_api/
ifeq "$(CONFIG_RTK_VOIP_DRIVERS_SLIC_LINETEST)" "y"
EXTRA_CFLAGS += -I$(DIR_VOIP)/voip_drivers/zarlink/$(ZARLINK_LIB)/linetest/vp890_lt/
endif
endif

EXTRA_CFLAGS += -I$(DIR_VOIP)/voip_drivers/zarlink/
EXTRA_CFLAGS += -I$(DIR_VOIP)/voip_drivers/zarlink/common/
EXTRA_CFLAGS += -I$(DIR_VOIP)/voip_drivers/zarlink/$(ZARLINK_LIB)/includes/
ifeq "$(CONFIG_RTK_VOIP_DRIVERS_SLIC_LINETEST)" "y"
EXTRA_CFLAGS += -I$(DIR_VOIP)/voip_drivers/zarlink/$(ZARLINK_LIB)/linetest/include/
endif
EXTRA_CFLAGS += -I$(DIR_VOIP)/voip_drivers/zarlink/$(ZARLINK_LIB)/$(ZARLINK_PROFILE)/
EXTRA_CFLAGS += -I$(DIR_VOIP)/voip_drivers/zarlink/$(ZARLINK_LIB)/arch/rtl89xxb/

EXTRA_CFLAGS += -I$(DIR_VOIP)/include/
EXTRA_CFLAGS += -I$(DIR_VOIP)/voip_drivers/
EXTRA_CFLAGS += -I$(DIR_VOIP)/voip_drivers/spi/

include $(DIR_VOIP)/rules.mak
