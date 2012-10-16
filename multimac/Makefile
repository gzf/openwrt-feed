# 
# Author: Zhifeng Gu <guzhifeng1979@hotmail.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=multimac
PKG_VERSION:=1.0.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME).tar.gz
PKG_SOURCE_URL:=http://sourceforge.net/projects/multimac/files/multimac/$(PKG_VERSION)
PKG_MD5SUM:=a861b16f7f96da6e846c9c0cf5d108a0

include $(INCLUDE_DIR)/package.mk

define Package/multimac
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Network Devices
  TITLE:=Multiple MACs on an adapter
  URL:=http://sourceforge.net/projects/multimac/
  MAINTAINER:=Zhifeng Gu <guzhifeng1979@hotmail.com>
  DEPENDS+= +kmod-tun +libpthread
endef

define Package/multimac/description
  Multimac is a very simple Linux software that is able to emulate
  and use multiple virtual interfaces (with different MAC addresses)
  on a LAN using a single network adapter. The aim of the application
  is giving network developers a tool to emulate presence of multiple
  machines (with different MAC addresses) using a single network adapter.
endef

define Build/Prepare
	gzip -dc $(DL_DIR)/$(PKG_SOURCE) | tar -C $(PKG_BUILD_DIR) -x --strip-components=1
	rm -f $(PKG_BUILD_DIR)/multimac
	$(Build/Patch)
endef

define Package/multimac/conffiles
/etc/config/multimac
endef

define Package/multimac/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/multimac $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/multimac.init $(1)/etc/init.d/mmac
	$(INSTALL_DIR) $(1)/etc/config/
	$(INSTALL_DATA) ./files/multimac.conf $(1)/etc/config/mmac
endef

$(eval $(call BuildPackage,multimac))
