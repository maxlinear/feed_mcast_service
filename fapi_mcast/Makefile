#
# Copyright © 2020-2023 MaxLinear, Inc.
# Openwrt Makefile for libmcastfapi
#

include $(TOPDIR)/rules.mk
PKG_NAME:=libmcastfapi
PKG_SOURCE_VERSION:=3.2.1.15
PKG_VERSION:=$(PKG_SOURCE_VERSION)
PKG_SOURCE_PROTO:=git
PKG_PROJECT:=maxlinear
PKG_SOURCE_NAME:=fapi_mcast
CONFIG_UGW_PKG_SOURCE_URL:=https://github.com
PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.xz
PKG_SOURCE_URL:=$(CONFIG_UGW_PKG_SOURCE_URL)/$(PKG_PROJECT)/$(PKG_SOURCE_NAME).git
PKG_MIRROR_HASH:=0a1f790481c2ca425f7c4b7dda4902fae651064515c6ef9258327b9e91ab970a
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_SOURCE_VERSION)
PKG_MAINTAINER:=MaxLinear
PKG_LICENSE:=MaxLinear
PKG_MXL_LICENSE:=MaxLinear/LICENSE

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk
-include $(STAGING_DIR)/mk/ugw-loglevel.mk
-include $(INCLUDE_DIR)/package-version-override.mk

define Package/libmcastfapi
  SECTION:=MaxLinear
  CATEGORY:=MaxLinear
  SUBMENU:=UGW Software
  TITLE:=MaxLinear Multicast FAPI
  URL:=http://www.maxlinear.com
  MAINTAINER:=MaxLinear Inc
  DEPENDS:=+kmod-mcast-helper +libsafec3-7
  MENU:=1
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/mcast
	$(INSTALL_DIR) $(1)/usr/lib/mcast
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/include/*.h $(1)/usr/include/mcast/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/libmcastfapi.so $(1)/usr/lib/mcast/
	$(INSTALL_DIR) $(1)/usr/docs
	$(CP) $(PKG_BUILD_DIR)/include/fapi_mcast.h $(1)/usr/docs/.
endef

define Package/libmcastfapi/install
	$(INSTALL_DIR) $(1)$(VENDOR_PATH)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libmcastfapi.so $(1)$(VENDOR_PATH)/usr/lib/
endef

$(eval $(call BuildPackage,libmcastfapi))
