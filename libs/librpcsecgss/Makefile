# 
# Copyright (C) 2006-2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=librpcsecgss
PKG_VERSION:=0.19
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://www.citi.umich.edu/projects/nfsv4/linux/librpcsecgss
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_HASH:=0cafb86b67e5eb4c89e8abaaad9165298946bc164d258e8925fc6dc1fa913abd

PKG_MAINTAINER:=Marcin Jurkowski <marcin1j@gmail.com>

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/librpcsecgss
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=implementation of rpcsec_gss (RFC 2203) for secure rpc communication
  URL:=http://www.citi.umich.edu/projects/nfsv4/linux/
  DEPENDS:=+libgssglue $(LIBRPC_DEPENDS)
endef

define Package/librpcsecgss/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/librpcsecgss.so* $(1)/usr/lib
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/librpcsecgss.{a,so*} $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/librpcsecgss.pc $(1)/usr/lib/pkgconfig/librpcsecgss.pc
endef

$(eval $(call BuildPackage,librpcsecgss))
