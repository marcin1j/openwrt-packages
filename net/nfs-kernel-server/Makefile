# Copyright (C) 2009-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=nfs-kernel-server
PKG_VERSION:=2.3.1
PKG_RELEASE:=1
PKG_HASH:=ca92f1ab86b2af4dcd62d7716d46a6cdec268e83fe8d564cd8ff1464cc495989

PKG_SOURCE_URL:=@SF/nfs
PKG_SOURCE:=nfs-utils-$(PKG_VERSION).tar.bz2

HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/nfs-utils-$(PKG_VERSION)
PKG_BUILD_DIR:=$(BUILD_DIR)/nfs-utils-$(PKG_VERSION)
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_BUILD_DEPENDS:=$(PKG_NAME)/host keyutils krb5 libtirpc

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/nfs-kernel-server/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Filesystem
  DEPENDS:=+libwrap +libblkid +libuuid $(LIBRPC_DEPENDS) +libtirpc
  URL:=http://nfs.sourceforge.net/
  MAINTAINER:=Peter Wagner <tripolar@gmx.at>
endef

define Package/nfs-utils/Default
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS+= +libevent2 +USE_UCLIBC:librpc $(LIBRPC_DEPENDS)
  TITLE:=NFS utils:
endef

define Package/nfs-utils-daemon/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Filesystem
  DEPENDS:=+libevent2 $(LIBRPC_DEPENDS)
  URL:=http://nfs.sourceforge.net/
  TITLE:=NFS client and server daemons
  MAINTAINER:=Peter Wagner <tripolar@gmx.at>
endef

define Package/nfs-kernel-server
  $(call Package/nfs-kernel-server/Default)
  TITLE:=Kernel NFS server support
  DEPENDS+= +kmod-fs-nfsd +kmod-fs-nfs +portmap
endef

define Package/nfs-kernel-server/description
  Kernel NFS server support
endef

define Package/nfs-kernel-server/conffiles
/etc/exports
endef

define Package/nfs-kernel-server-utils
  $(call Package/nfs-kernel-server/Default)
  TITLE:=NFS server utils
  DEPENDS:=nfs-utils-showmount nfs-utils-nfsstat
endef

define Package/nfs-kernel-server-utils/description
  NFS server utils
endef


TARGET_CFLAGS += -I$(PKG_BUILD_DIR)/lib -I$(STAGING_DIR)/usr/include/libevent \
		 -I$(STAGING_DIR)/usr/include/ -Drpc_uint=uint
TARGET_LDFLAGS += -Wl,-rpath-link=$(STAGING_DIR)/usr/lib $(LIBRPC) \
		  -L$(STAGING_DIR)/usr/lib/libevent

CONFIGURE_ARGS += \
	--enable-gss \
	--enable-nfsv4 \
	--disable-nfsv41 \
	--disable-ipv6 \
	--enable-static \
	--enable-shared \
	--disable-caps \
	--enable-tirpc \
	--enable-svcgss \
	--disable-nfsdcld \
	--disable-nfsdcltrack

CONFIGURE_VARS += \
	libblkid_cv_is_recent=yes \
	ac_cv_lib_resolv___res_querydomain=yes \
	CONFIG_SQLITE3_TRUE="\#" \
	CONFIG_NFSDCLD_TRUE="\#"

MAKE_FLAGS += \
	OPT="$(TARGET_CFLAGS)" \
	INSTALLSUID="install -m 4755" \
	DESTDIR="$(PKG_INSTALL_DIR)" \
	RPCGEN_PATH=$(STAGING_DIR_HOSTPKG)/bin/rpcgen \
	RPCGEN=$(STAGING_DIR_HOSTPKG)/bin/rpcgen

HOST_CFLAGS += -Dlinux

HOST_CONFIGURE_ARGS += \
	--disable-gss \
	--disable-nfsv4 \
	--disable-nfsv41 \
	--disable-ipv6 \
	--disable-tirpc \
	--without-tcp-wrappers

HOST_CONFIGURE_VARS += \
	ac_cv_lib_event_event_dispatch=yes \
	ac_cv_lib_nfsidmap_nfs4_init_name_mapping=yes \
	ac_cv_lib_blkid_blkid_get_library_version=yes \
	ac_cv_header_event_h=yes \
	ac_cv_header_nfsidmap_h=yes \
	ac_cv_header_blkid_blkid_h=yes \
	GSSGLUE_CFLAGS=" " \
	GSSGLUE_LIBS=" " \
	RPCSECGSS_CFLAGS=" " \
	RPCSECGSS_LIBS=" " \
	CONFIG_SQLITE3_TRUE="\#" \
	CONFIG_NFSDCLD_TRUE="\#"

define Host/Compile
	$(MAKE) -C $(HOST_BUILD_DIR)/tools/rpcgen all
endef

define Host/Install
	$(INSTALL_DIR) $(STAGING_DIR_HOSTPKG)/bin
	$(INSTALL_BIN) $(HOST_BUILD_DIR)/tools/rpcgen/rpcgen $(STAGING_DIR_HOSTPKG)/bin/rpcgen
endef

define Package/nfs-kernel-server/install
	$(INSTALL_DIR) $(1)/etc/init.d $(1)/usr/sbin
	$(INSTALL_DATA) ./files/nfsd.exports $(1)/etc/exports
	$(INSTALL_BIN) ./files/nfsd.init $(1)/etc/init.d/nfsd
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/utils/statd/sm-notify $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/utils/statd/statd $(1)/usr/sbin/rpc.statd
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/utils/nfsd/nfsd $(1)/usr/sbin/rpc.nfsd
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/utils/mountd/mountd $(1)/usr/sbin/rpc.mountd
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/utils/exportfs/exportfs $(1)/usr/sbin/
endef

define Package/nfs-utils-nfsstat
  $(call Package/nfs-utils/Default)
  TITLE+=/usr/sbin/nfsstat
endef
define Package/nfs-utils-nfsstat/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/utils/nfsstat/nfsstat $(1)/usr/sbin
endef


define Package/nfs-utils-showmount
  $(call Package/nfs-utils/Default)
  TITLE+=/usr/sbin/showmount
  DEPENDS+=+libtirpc
endef
define Package/nfs-utils-showmount/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/utils/showmount/showmount $(1)/usr/sbin
endef

define Package/nfs-utils-mount
  $(call Package/nfs-utils/Default)
  TITLE+=/sbin/mount.nfs and /sbin/mount.nfs4
  DEPENDS+=+libtirpc
endef
define Package/nfs-utils-mount/install
	$(INSTALL_DIR) $(1)/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/sbin/mount.nfs $(1)/sbin/
	(cd $(1)/sbin; ln -sf mount.nfs mount.nfs4; ln -sf mount.nfs umount.nfs; ln -sf mount.nfs umount.nfs4)
endef

define Package/nfs-utils-mount/description
  Updated mount.nfs command - allows mounting nfs4 volumes
endef

define Package/nfs-utils-rpc-pipefs
  $(call Package/nfs-utils-daemon/Default)
  TITLE+=rpc.pipefs
  DEPENDS:=+kmod-fs-nfs-common
endef

define Package/nfs-utils-rpc-pipefs/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/rpc.pipefs.init $(1)/etc/init.d/rpc.pipefs
endef

define Package/nfs-utils-rpc-idmapd
  $(call Package/nfs-utils-daemon/Default)
  TITLE+=rpc.idmapd
  DEPENDS+=+portmap +nfs-utils-rpc-pipefs
endef

define Package/nfs-utils-rpc-idmapd/conffiles
/etc/idmapd.conf
endef

define Package/nfs-utils-rpc-idmapd/install
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/support/nfsidmap/idmapd.conf $(1)/etc/idmapd.conf

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/rpc.idmapd.init $(1)/etc/init.d/rpc.idmapd

	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/rpc.idmapd $(1)/usr/sbin/rpc.idmapd

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libnfsidmap.so* $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/libnfsidmap
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libnfsidmap/*.so* $(1)/usr/lib/libnfsidmap
endef

define Package/nfs-utils-rpc-gssd
  $(call Package/nfs-utils-daemon/Default)
  TITLE+=rpc.gssd
  DEPENDS+=+libtirpc +krb5-libs +portmap +nfs-utils-rpc-pipefs
endef

define Package/nfs-utils-rpc-gssd/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/rpc.gssd.init $(1)/etc/init.d/rpc.gssd

	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/rpc.gssd $(1)/usr/sbin/rpc.gssd
endef

define Package/nfs-utils-rpc-svcgssd
  $(call Package/nfs-utils-daemon/Default)
  TITLE+=rpc.svcgssd
  DEPENDS+=+libtirpc +krb5-libs +portmap +nfs-utils-rpc-idmapd
endef

define Package/nfs-utils-rpc-svcgssd/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/rpc.svcgssd.init $(1)/etc/init.d/rpc.svcgssd

	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/rpc.svcgssd $(1)/usr/sbin/rpc.svcgssd
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,nfs-kernel-server))
$(eval $(call BuildPackage,nfs-kernel-server-utils))

$(eval $(call BuildPackage,nfs-utils-nfsstat))
$(eval $(call BuildPackage,nfs-utils-showmount))
$(eval $(call BuildPackage,nfs-utils-mount))
$(eval $(call BuildPackage,nfs-utils-rpc-pipefs))
$(eval $(call BuildPackage,nfs-utils-rpc-idmapd))
$(eval $(call BuildPackage,nfs-utils-rpc-gssd))
$(eval $(call BuildPackage,nfs-utils-rpc-svcgssd))
