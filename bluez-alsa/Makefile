#
# Copyright (C) 2017-2018 Daniel Engberg <daniel.engberg.lists@pyret.net>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=bluez-alsa
PKG_VERSION:=20220601.f3b84e9
PKG_RELEASE:=$(AUTORELEASE)
PKG_MAINTAINER:=
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE.txt

#PKG_SOURCE_URL:=https://codeload.github.com/Arkq/$(PKG_NAME)/tar.gz/fbbd2a6?dummy=/
#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
#PKG_HASH:=39eca3c2fc7c50abc423b1236a2cb3e991e224895652e3f9f71ee7f5cc1b8a23
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
#-fbbd2a6

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/schavand/bluez-alsa.git
PKG_SOURCE_DATE:=2022-06-01
PKG_SOURCE_VERSION:=f3b84e924733ce3f44f8cf397974f74fca272e6a
#PKG_MIRROR_HASH:=caedb66cf6dcbdcee0d1525923e203d003ef15f34a13a328686794666f16171f

PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/bluez-alsa
  SECTION:=sound
  CATEGORY:=Sound
  DEPENDS:=+alsa-lib +bluez-daemon +glib2 +sbc +fdk-aac
  TITLE:=Bluetooth Audio ALSA backend
  URL:=https://github.com/schavand/bluez-alsa
endef

define Package/bluez-alsa/config
  source "$(SOURCE)/Config.in"
endef

define Package/bluez-alsa/description
  ALSA plugin supporting Bluetooth audio
endef

CONFIGURE_ARGS += --enable-aac --disable-rfcomm

define Package/bluez-alsa/install
	$(INSTALL_DIR) $(1)/etc/alsa/conf.d/
	$(CP) $(PKG_BUILD_DIR)/src/asound/20-bluealsa.conf $(1)/etc/alsa/conf.d/
	$(INSTALL_DIR) $(1)/usr/lib/alsa-lib
	$(CP) $(PKG_BUILD_DIR)/src/asound/.libs/libasound_module_*_bluealsa.so $(1)/usr/lib/alsa-lib
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/bluealsa $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/utils/aplay/bluealsa-aplay $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/bluealsa.init $(1)/etc/init.d/bluealsa
ifeq ($(CONFIG_BLUEALSA_A2DP_SOURCE),y)
	$(SED) 's/-i hci0/-i hci0 -p a2dp-source/g' $(1)/etc/init.d/bluealsa
endif
ifeq ($(CONFIG_BLUEALSA_A2DP_SINK),y)
	$(SED) 's/-i hci0/-i hci0 -p a2dp-sink/g' $(1)/etc/init.d/bluealsa
endif
ifeq ($(BLUEALSA_HFP_OFONO),y)
	$(SED) 's/-i hci0/-i hci0 -p hfp-ofono/g' $(1)/etc/init.d/bluealsa
endif
ifeq ($(BLUEALSA_HFP_AG),y)
	$(SED) 's/-i hci0/-i hci0 -p hfp-ag/g' $(1)/etc/init.d/bluealsa
endif
ifeq ($(BLUEALSA_HFP_HF),y)
	$(SED) 's/-i hci0/-i hci0 -p hfp-hf/g' $(1)/etc/init.d/bluealsa
endif
ifeq ($(BLUEALSA_HSP_AG),y)
	$(SED) 's/-i hci0/-i hci0 -p hsp-ag/g' $(1)/etc/init.d/bluealsa
endif
ifeq ($(BLUEALSA_HSP_HS),y)
	$(SED) 's/-i hci0/-i hci0 -p hsp-hs/g' $(1)/etc/init.d/bluealsa
endif
	$(INSTALL_DIR) $(1)/etc/dbus-1/system.d/
	$(CP) $(PKG_BUILD_DIR)/src/bluealsa.conf $(1)/etc/dbus-1/system.d/
endef

$(eval $(call BuildPackage,bluez-alsa))
