# SPDX-License-Identifier: Apache-2.0
#
# Copyright (C) 2023 ImmortalWrt.org

include $(TOPDIR)/rules.mk

# Keep package and translation versions stable when copied into an SDK.
PKG_VERSION:=1.0.1
PKG_RELEASE:=3
PKG_PO_VERSION:=$(PKG_VERSION)-r$(PKG_RELEASE)

LUCI_TITLE:=LuCI app for qBittorrent
LUCI_DEPENDS:=+qbittorrent-enhanced-edition
LUCI_PKGARCH:=all

include $(TOPDIR)/feeds/luci/luci.mk

# call BuildPackage - OpenWrt buildroot signature
