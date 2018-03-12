# 
# Copyright (C) 2006-2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libgssglue
PKG_VERSION:=0.4
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://www.citi.umich.edu/projects/nfsv4/linux/libgssglue
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_HASH:=3f791a75502ba723e5e85e41e5e0c711bb89e2716b7c0ec6e74bd1df6739043a

PKG_MAINTAINER:=Marcin Jurkowski <marcin1j@gmail.com>

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libgssglue
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=exports a gssapi interface which calls other random gssapi libraries
  URL:=http://www.citi.umich.edu/projects/nfsv4/linux/
  DEPENDS:=
endef

define Package/libgssglue/conffiles
/etc/gssapi_mech.conf
endef

define Package/libgssglue/install
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/doc/gssapi_mech.conf $(1)/etc/gssapi_mech.conf

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libgssglue.so* $(1)/usr/lib
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libgssglue.{a,so*} $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libgssglue.pc $(1)/usr/lib/pkgconfig/libgssglue.pc
endef

$(eval $(call BuildPackage,libgssglue))
