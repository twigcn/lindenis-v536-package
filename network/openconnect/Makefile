#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=openconnect
PKG_VERSION:=7.06
PKG_RELEASE:=3
PKG_USE_MIPS16:=0

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=ftp://ftp.infradead.org/pub/openconnect/
PKG_MD5SUM:=80f397911e1fed43d897d99be3d5f1a1

PKG_CONFIG_DEPENDS:= \
	CONFIG_OPENCONNECT_GNUTLS \
	CONFIG_OPENCONNECT_OPENSSL \


include $(BUILD_DIR)/package.mk

define Package/openconnect/config
	source "$(SOURCE)/Config.in"
endef

define Package/openconnect
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libxml2 +kmod-tun +resolveip +vpnc-scripts +OPENCONNECT_OPENSSL:libopenssl +OPENCONNECT_GNUTLS:libgnutls +OPENCONNECT_STOKEN:libstoken
  TITLE:=OpenConnect VPN client (Cisco AnyConnect compatible)
  MAINTAINER:=Nikos Mavrogiannopoulos <n.mavrogiannopoulos@gmail.com>
  URL:=http://www.infradead.org/openconnect/
  SUBMENU:=VPN
endef

define Package/openconnect/description
	A VPN client compatible with Cisco's AnyConnect SSL VPN and ocserv.

        OpenConnect is a client that follows the Cisco's AnyConnect SSL VPN protocol,
        which is supported by IOS 12.4(9)T or later on Cisco SR500, 870, 880, 1800, 
        2800, 3800, 7200 Series and Cisco 7301 Routers, as well as the OpenConnect
        VPN server.
endef

CONFIGURE_ARGS += \
	--disable-shared \
	--with-vpnc-script=/lib/netifd/vpnc-script \
	--without-libpcsclite \
	--without-stoken

ifeq ($(CONFIG_OPENCONNECT_OPENSSL),y)
CONFIGURE_ARGS += \
	--without-gnutls
endif

ifeq ($(CONFIG_OPENCONNECT_STOKEN),y)
CONFIGURE_ARGS += \
	--with-stoken
endif

define Package/openconnect/install
	$(INSTALL_DIR) $(1)/etc/openconnect/
	$(INSTALL_DIR) $(1)/lib/netifd/proto
	$(INSTALL_BIN) ./files/openconnect.sh $(1)/lib/netifd/proto/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/openconnect $(1)/usr/sbin/
	$(INSTALL_BIN) ./files/openconnect-wrapper $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/lib/upgrade/keep.d
	$(INSTALL_DATA) ./files/openconnect.upgrade $(1)/lib/upgrade/keep.d/openconnect
endef

$(eval $(call BuildPackage,openconnect))
