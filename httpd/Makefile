#
# Makefile for http stuff
#
obj-y += uip.o
obj-y += uip_arch.o
obj-y += uip_arp.o
obj-y += httpd.o
obj-y += fs.o

HTTPD_VENDOR ?= cleanwrt
HTTPD_VENDOR_DIR := $(srctree)/httpd/vendors/$(HTTPD_VENDOR)
HTTPD_ASSETS := $(wildcard $(HTTPD_VENDOR_DIR)/*)

$(src)/fsdata.c: $(HTTPD_ASSETS) $(srctree)/httpd/vendors/makefsdatac
	$(Q)cd $(srctree)/httpd && ./vendors/makefsdatac $(HTTPD_VENDOR)

$(obj)/fs.o: $(src)/fsdata.c
CFLAGS_uip.o  +=  -fno-strict-aliasing
CFLAGS_uip_arch.o  +=  -fno-strict-aliasing
CFLAGS_uip_arp.o  +=  -fno-strict-aliasing
