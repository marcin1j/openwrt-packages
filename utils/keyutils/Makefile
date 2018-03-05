# 
# Copyright (C) 2006-2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=keyutils
PKG_VERSION:=1.5.10
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://people.redhat.com/dhowells/keyutils/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_HASH:=115c3deae7f181778fd0e0ffaa2dad1bf1fe2f5677cf2e0e348cdb7a1c93afb6

PKG_MAINTAINER:=Marcin Jurkowski <marcin1j@gmail.com>

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/keyutils/Default
  TITLE:=Linux Key Management Utilities
  URL:=https://people.redhat.com/dhowells/keyutils/
endef

define Package/keyutils
$(call Package/keyutils/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE+=utils
  DEPENDS:=+libkeyutils
endef

define Package/libkeyutils
$(call Package/keyutils/Default)
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE+=library
  DEPENDS:=
endef

define Package/keyutils/install
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/etc/request-key.conf $(1)/etc

	$(INSTALL_DIR) $(1)/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/keyctl $(1)/bin

	$(INSTALL_DIR) $(1)/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/sbin/request-key $(1)/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/sbin/key.dns_resolver $(1)/sbin

	$(INSTALL_DIR) $(1)/usr/share/keyutils
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/keyutils/request-key-debug.sh $(1)/usr/share/keyutils
endef

define Package/keyutils/conffiles
/etc/request-key.conf
endef

define Package/libkeyutils/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/lib*/libkeyutils.so* $(1)/usr/lib
endef

TARGET_CFLAGS += $(FPIC)

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/lib*/libkeyutils.{a,so*} $(1)/usr/lib
endef

$(eval $(call BuildPackage,keyutils))
$(eval $(call BuildPackage,libkeyutils))
