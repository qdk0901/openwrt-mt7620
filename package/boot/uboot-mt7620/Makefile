#
# Copyright (C) 2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=u-boot
PKG_VERSION:=2010.03
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)
PKG_MD5SUM:=
PKG_TARGETS:=bin

include $(INCLUDE_DIR)/package.mk

define uboot/Default
  TITLE:=
  CONFIG:=
  IMAGE:=
endef

define uboot/xdr
  TITLE:=U-boot for xiaodu router
endef

UBOOTS:=xdr

define Package/uboot/template
define Package/uboot-mt7620-$(1)
  SECTION:=boot
  CATEGORY:=Boot Loaders
  TITLE:=$(2)
  DEPENDS:=@TARGET_ramips
  URL:=http://www.denx.de/wiki/U-Boot
  DEFAULT:=y if (CONFIG_TARGET_ramips_mt7620n || CONFIG_TARGET_ramips_mt7620a)
  VARIANT:=$(1)
endef
endef

define BuildUbootPackage
	$(eval $(uboot/Default))
	$(eval $(uboot/$(1)))
	$(call Package/uboot/template,$(1),$(TITLE))
endef


ifdef BUILD_VARIANT
$(eval $(call uboot/$(BUILD_VARIANT)))
UBOOT_CONFIG:=$(if $(CONFIG),$(CONFIG),$(BUILD_VARIANT))
UBOOT_IMAGE:=$(if $(IMAGE),$(IMAGE),openwrt-$(BOARD)-$(BUILD_VARIANT)-u-boot.bin)
endif

define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) -r src/. $(PKG_BUILD_DIR)
endef

define Build/Configure
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CROSS_COMPILE=$(TARGET_CROSS)
endef

define Package/uboot/install/template
define Package/uboot-mt7620-$(1)/install
	$(INSTALL_DIR) $$(1)
	$(CP) $(PKG_BUILD_DIR)/uboot.bin $(BIN_DIR)/$(2)
endef
endef

$(foreach u,$(UBOOTS), \
	$(eval $(call Package/uboot/install/template,$(u),openwrt-$(BOARD)-$(SUBTARGET)-$(u)-u-boot.bin)) \
)

$(foreach u,$(UBOOTS), \
	$(eval $(call BuildUbootPackage,$(u))) \
	$(eval $(call BuildPackage,uboot-mt7620-$(u))) \
)
