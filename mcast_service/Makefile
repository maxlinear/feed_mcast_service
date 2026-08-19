#
# Copyright © 2020-2024 MaxLinear, Inc.
# Makefile for libmcastdaemon
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mcast_service
PKG_SOURCE_VERSION:=3.1.2.21
PKG_VERSION:=$(PKG_SOURCE_VERSION)
PKG_SOURCE_PROTO:=git
PKG_PROJECT:=maxlinear
PKG_SOURCE_NAME:=mcast_service
CONFIG_UGW_PKG_SOURCE_URL:=https://github.com
PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.xz
PKG_SOURCE_URL:=$(CONFIG_UGW_PKG_SOURCE_URL)/$(PKG_PROJECT)/$(PKG_SOURCE_NAME).git
PKG_MIRROR_HASH:=7e3e8c482de7b632f3fa7f9d8cab19d8033c5b4996b99b340daf8e9574df2636
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_SOURCE_VERSION)
PKG_MAINTAINER:=MaxLinear
PKG_LICENSE:=MaxLinear
PKG_MXL_LICENSE:=MaxLinear/LICENSE
PKG_CONFIG_DEPENDS:=CONFIG_IFX_CONFIG_CPU 

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk
-include $(INCLUDE_DIR)/package-version-override.mk
-include $(STAGING_DIR)/mk/ugw-loglevel.mk

define Package/$(PKG_NAME)/Default
  SECTION:=utils
  CATEGORY:=MaxLinear
  SUBMENU:=UGW Software
  URL:=http://www.maxlinear.com
  MAINTAINER:=MaxLinear Inc
endef

define Package/$(PKG_NAME)
  $(call Package/$(PKG_NAME)/Default)
  TITLE:=MaxLinear Multicast Services
  MENU:=1
endef

define Package/$(PKG_NAME)/description
  MaxLinear Multicast Services
endef

define Package/mcastcli
  $(call Package/$(PKG_NAME)/Default)
  TITLE:=MaxLinear Multicast Cli
  DEPENDS:=+$(PKG_NAME) +libsafec3-7 +libmcastfapi
endef

define Package/mcastcli/description
  MaxLinear Multicast Cli Support.
endef

define Package/libmcastdaemon
  $(call Package/$(PKG_NAME)/Default)
  TITLE:=MaxLinear multicast proxy daemon
  DEPENDS:=+$(PKG_NAME) +kmod-mcast-helper +libmcastfapi +libsafec3-7 +librt
  VARIANT:=release
endef

define Package/libmcastdaemon/description
  MaxLinear Multicast Proxy Daemon Support.
endef

define Package/libmcastdaemon-dbg
  $(call Package/libmcastdaemon)
  TITLE:=MaxLinear multicast proxy daemon with debug
  VARIANT:=debug
endef

define Package/libmcastdaemon-dbg/description
  MaxLinear Multicast Proxy Daemon Support with debug
endef

# Export PKG_VERSION to be used in mcastd
export PKG_VERSION

ifneq ($(CONFIG_PACKAGE_libmcastdaemon),)
  export CONFIG_MCASTD=1
endif

ifneq ($(CONFIG_PACKAGE_mcastcli),)
  export CONFIG_MCASTCLI=1
endif

ifneq ($(CONFIG_PACKAGE_kmod-mcast-snooping),)
  export CONFIG_MCAST_SNOOPING=1
endif

define Build/Prepare
	$(call Build/Prepare/Default)
	$(INSTALL_DIR) $(STAGING_DIR)/usr/include
	touch $(STAGING_DIR)/usr/include/vendor.h
	echo "#define VENDOR_PATH \"$(VENDOR_PATH)\"" > $(STAGING_DIR)/usr/include/vendor.h
endef

# To ipkg / Rootfs
define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/etc/
endef

define Package/mcastcli/install
	$(INSTALL_DIR) $(1)$(VENDOR_PATH)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/mcastcli/mcastcli $(1)/$(VENDOR_PATH)/usr/sbin
endef

define Package/libmcastdaemon/install
	$(INSTALL_DIR) $(1)$(VENDOR_PATH)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/mcastd/mcastd $(1)$(VENDOR_PATH)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/multicast.init $(1)/etc/init.d/multicast
	$(INSTALL_DIR) $(1)/etc/hotplug.d/iface
	$(INSTALL_DATA) ./files/multicast.hotplug $(1)/etc/hotplug.d/iface/11-multicast
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/mcast.config $(1)/etc/config/mcast
	$(INSTALL_DIR) $(1)/usr/share/capabilities
	$(INSTALL_DIR) $(1)/usr/share/pseudofs_acl
	$(INSTALL_DATA) ./files/multicast.json $(1)/usr/share/capabilities/multicast.json
	$(INSTALL_DATA) ./files/mcastd_pseudofs_acl.json $(1)/usr/share/pseudofs_acl/mcastd_pseudofs_acl.json
endef

Package/libmcastdaemon-dbg/install = $(Package/libmcastdaemon/install)

define Build/Clean
	if [ -d $(PKG_BUILD_DIR) ]; then \
		$(MAKE) -C $(PKG_BUILD_DIR) clean; \
		$(RM) -rf $(PKG_BUILD_DIR)/ipkg-$(PKGARCH); \
		$(RM) -rf $(PKG_BUILD_DIR)/{.pkgdir,.built,.configured_*}; \
		$(RM) -rf $(PKG_BUILD_DIR)/{.built_*,.dep_files,.prepared*}; \
	fi
endef

$(eval $(call BuildPackage,mcastcli))
$(eval $(call BuildPackage,libmcastdaemon))
$(eval $(call BuildPackage,libmcastdaemon-dbg))
$(eval $(call BuildPackage,$(PKG_NAME)))
