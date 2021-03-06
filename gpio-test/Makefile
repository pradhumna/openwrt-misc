#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=gpio-test
PKG_RELEASE:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define KernelPackage/$(PKG_NAME)
  SUBMENU:=Other modules
  TITLE:=Simple GPIO Test
  DEPENDS:=
  FILES:=$(PKG_BUILD_DIR)/{gpio-test.ko,gpio-irq-test.ko,gpio-toggle-test.ko,gpio-hw-irq-latency-test.ko,gpio-irq-latency-test.ko}
  KCONFIG:=
endef

define KernelPackage/$(PKG_NAME)/description
 Simple GPIO test suite including 'toggle speed test' and 'GPIO IRQ latency test'
endef

EXTRA_KCONFIG:= \
    CONFIG_GPIO_TEST=m

EXTRA_CFLAGS:= \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=m,%,$(filter %=m,$(EXTRA_KCONFIG)))) \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=y,%,$(filter %=y,$(EXTRA_KCONFIG)))) \

ifneq (,$(findstring mt76x8,$(CONFIG_TARGET_SUBTARGET)))
        EXTRA_CFLAGS+= -DMT7688
endif

ifneq (,$(findstring mt7688,$(CONFIG_TARGET_SUBTARGET)))
        EXTRA_CFLAGS+= -DMT7688
endif

ifneq (,$(findstring rt305x,$(CONFIG_TARGET_SUBTARGET)))
        EXTRA_CFLAGS+= -DRT305X
endif

ifneq (,$(findstring ar71xx,$(CONFIG_TARGET_BOARD)))
        EXTRA_CFLAGS+= -DATH79_SOC
endif

MAKE_OPTS:= \
	$(KERNEL_MAKE_FLAGS) \
	M="$(PKG_BUILD_DIR)" \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
	$(EXTRA_KCONFIG)

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" \
		$(MAKE_OPTS) \
		modules
endef

$(eval $(call KernelPackage,$(PKG_NAME)))
