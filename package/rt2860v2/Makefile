#
# Copyright (C) 2006-2009 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=rt2860v2

PKG_REV:=2714
PKG_VERSION:=r$(PKG_REV)
PKG_RELEASE:=6

PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define KernelPackage/rt2860v2
  SUBMENU:=Wireless Drivers
  TITLE:=Driver for MT7620
  DEFAULT:=
  DEPENDS:=@TARGET_ramips
  FILES:=$(PKG_BUILD_DIR)/rt2860v2_ap/rt2860v2_ap.ko
  AUTOLOAD:=$(call AutoLoad,50,rt2860v2_ap)
endef

define KernelPackage/rt2860v2/description
 This package contains a driver for MT7620
endef

define KernelPackage/rt2860v2/config
	source "$(SOURCE)/files/rt2860v2_ap/Kconfig"
endef

SOURCE_DIR:=$(PKG_BUILD_DIR)
export SOURCE_DIR

MAKEOPTS:= -C $(LINUX_DIR) \
                ARCH="$(LINUX_KARCH)" \
                CROSS_COMPILE="$(TARGET_CROSS)" \
                M="$(PKG_BUILD_DIR)/rt2860v2_ap" \
                modules

define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) -r files/. $(PKG_BUILD_DIR)
endef

CONFIGS:=$(shell sed -n 's/CONFIG_RT2860V2/CONFIG_RT2860V2/p' $(TOPDIR)/.config | sed -e '/^\#/d' | sed ':a;N;s/\n/ /g;ta')

define Build/Compile
	$(MAKE) $(CONFIGS) $(MAKEOPTS)
endef

define KernelPackage/rt2860v2/install
	$(INSTALL_DIR) $(1)/lib/wifi
	$(INSTALL_DATA) ./files/rt2860v2.sh $(1)/lib/wifi
endef

$(eval $(call KernelPackage,rt2860v2))
