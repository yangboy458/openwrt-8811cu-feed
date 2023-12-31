#
# Copyright (C) 2021 ImmortalWrt
# <https://immortalwrt.org>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8821cu-comfast
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/Pix13/8821cu-20210118.git
#PKG_SOURCE_URL:=https://github.com/brektrou/rtl8821CU.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2021-11-14
#PKG_SOURCE_VERSION:=ef3ff12118a75ea9ca1db8f4806bb0861e4fffef
PKG_SOURCE_VERSION:=2c2b9ae607abd0b9a42c26ac8a79d6e5c15f1fac
PKG_MIRROR_HASH:=2b43a454960dd9f69eb0b3648a73438148e38036bb9cd5e3c57ae5697110c18e

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINTER:=CN_SZTL <cnsztl@immortalwrt.org>

PKG_BUILD_PARALLEL:=1

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8821cu-comfast
  SUBMENU:=Wireless Drivers
  TITLE:=Comfast Realtek RTL8811CU/RTL8821CU support
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +kmod-mac80211 +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT
  FILES:=$(PKG_BUILD_DIR)/rtl8821cu-comfast.ko
  AUTOLOAD:=$(call AutoProbe,rtl8821cu-comfast)
  PROVIDES:=kmod-rtl8821cu-comfast
endef

NOSTDINC_FLAGS = \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-include backport/autoconf.h \
	-include backport/backport.h

EXTRA_KCONFIG:= \
	CONFIG_RTL8821CU-COMFAST=y \
	USER_MODULE_NAME=rtl8821cu-comfast

ifeq ($(ARCH),aarch64)
	EXTRA_KCONFIG += CONFIG_MP_VHT_HW_TX_MODE=n
endif

EXTRA_CFLAGS:= \
	-DRTW_SINGLE_WIPHY \
	-DRTW_USE_CFG80211_STA_EVENT \
	-DCONFIG_IOCTL_CFG80211 \
	-DCONFIG_CONCURRENT_MODE \
	-DBUILD_OPENWRT

ifeq ($(ARCH),arm)
	EXTRA_CFLAGS += -mfloat-abi=softfp
endif

ifeq ($(BOARD),x86)
	EXTRA_CFLAGS += -mhard-float
endif

MAKE_OPTS:= \
	$(KERNEL_MAKE_FLAGS) \
	M="$(PKG_BUILD_DIR)" \
	NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
	USER_EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
	$(EXTRA_KCONFIG)

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(MAKE_OPTS) \
		modules
endef

$(eval $(call KernelPackage,rtl8821cu-comfast))
