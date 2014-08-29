#
# Copyright (C) 2009-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=git
PKG_VERSION:=1.7.12.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=git://github.com/git/git.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_PROTO:=git

PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/git/Common
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Version Control Systems
  TITLE:=The fast version control system
  URL:=http://git-scm.com
endef

define Package/git
  $(call Package/git/Common)
  DEPENDS:=+libopenssl +libpthread
  TITLE+= (buildin commands)
endef

define Package/git-remote-curl
  $(call Package/git/Common)
  DEPENDS:=+git +libcurl +libexpat
  TITLE+= (http[s]/ftp[s] helpers)
endef

define Package/git-http-fetch
  $(call Package/git/Common)
  DEPENDS:=+git +libcurl
  TITLE+= (http-fetch)
endef

define Package/git-http-push
  $(call Package/git/Common)
  DEPENDS:=+git +libcurl +libexpat
  TITLE+= (http-push)
endef

FL-git-remote-curl := git-remote-http git-remote-https \
		 		 	 git-remote-ftp git-remote-ftps
FL-git-http-fetch := git-http-fetch
FL-git-http-push  := git-http-push

# compile configuration
MAKE_FLAGS := NO_INSTALL_HARDLINKS=YesPlease

# description configuration
define Package/git/description
 Git is a free & open source, distributed version control system
 designed to handle everything from small to very large projects
 with speed and efficiency. This package contains all buildin commands.
endef

define DescriptionTempalte
define Package/$(1)/description
 This package contains:
  $(FL-$(1))
endef
endef

$(eval $(call DescriptionTempalte,git-remote-curl))
$(eval $(call DescriptionTempalte,git-http-fetch))
$(eval $(call DescriptionTempalte,git-http-push))

# install configuration
GIT_CORE := usr/lib/git-core
GIT_CORE_SHARE := usr/share/git-core

define Package/git/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/git  $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/git-upload-pack  $(1)/usr/bin
	for FILE in $(PKG_INSTALL_DIR)/usr/bin/*; do \
		if [ -h $$$$FILE ] && [ `readlink $$$$FILE` = git ]; then \
			$(CP) $$$$FILE $(1)/usr/bin; \
		fi; \
	done
	$(INSTALL_DIR) $(1)/usr/lib
	for FILE in $(PKG_INSTALL_DIR)/usr/lib/lib*.so*; do \
		$(CP) $$$$FILE $(1)/usr/lib; \
	done
	$(INSTALL_DIR) $(1)/$(GIT_CORE)
	$(CP) $(PKG_INSTALL_DIR)/$(GIT_CORE)/git  $(1)/$(GIT_CORE)
	for FILE in $(PKG_INSTALL_DIR)/$(GIT_CORE)/*; do \
		if [ -h $$$$FILE ] && [ `readlink $$$$FILE` = git ]; then \
			$(CP) $$$$FILE $(1)/$(GIT_CORE); \
		fi; \
	done
	$(INSTALL_DIR) $(1)/$(GIT_CORE_SHARE)
	$(CP) $(PKG_INSTALL_DIR)/$(GIT_CORE_SHARE)/templates $(1)/$(GIT_CORE_SHARE)
endef

define InstallTemplate
define Package/$(1)/install
	$(INSTALL_DIR) $$(1)/$(GIT_CORE)
	for FILE in $(FL-$(1)); do \
		$(CP) $(PKG_INSTALL_DIR)/$(GIT_CORE)/$$$$$$$$FILE  $$(1)/$(GIT_CORE); \
	done
endef
endef

$(eval $(call InstallTemplate,git-remote-curl))
$(eval $(call InstallTemplate,git-http-fetch))
$(eval $(call InstallTemplate,git-http-push))

$(eval $(call BuildPackage,git))
$(eval $(call BuildPackage,git-remote-curl))
$(eval $(call BuildPackage,git-http-fetch))
$(eval $(call BuildPackage,git-http-push))
