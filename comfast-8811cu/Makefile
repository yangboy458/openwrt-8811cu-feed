include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8811cu-ct
PKG_RELEASE=2

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/ProKn1fe/RTL8811CU_OpenWRT.git
PKG_MIRROR_HASH:=7b306420645ba16942dbf304fe042417e560348ac8d928020fd00b9df4a70ddf
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=7445348faa8948a93f5b729250108a8a49b1d689

PKG_BUILD_PARALLEL:=1
#PKG_EXTMOD_SUBDIRS:=rtl8811cu-ct

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8811cu-ct
  SUBMENU:=Wireless Drivers
  TITLE:=Driver for Realtek 8811 CU devices comfast 912-ac, etc
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT
  FILES:=\
        $(PKG_BUILD_DIR)/rtl8811cu.ko
  AUTOLOAD:=$(call AutoProbe,rtl8811cu)
  PROVIDES:=kmod-rtl8811cu
endef

NOSTDINC_FLAGS = \
        -I$(PKG_BUILD_DIR) \
        -I$(PKG_BUILD_DIR)/include \
        -I$(STAGING_DIR)/usr/include/mac80211-backport \
        -I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
        -I$(STAGING_DIR)/usr/include/mac80211 \
        -I$(STAGING_DIR)/usr/include/mac80211/uapi \
        -include backport/backport.h

NOSTDINC_FLAGS+=-DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT -DBUILD_OPENWRT

define Build/Compile
        +$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
                $(KERNEL_MAKE_FLAGS) \
                M="$(PKG_BUILD_DIR)" \
                NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
                modules
endef

$(eval $(call KernelPackage,rtl8811cu-ct))
