include $(TOPDIR)/rules.mk

PKG_NAME:=dockerd
PKG_VERSION:=20.10.22
PKG_RELEASE:=5
PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/dockerd
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Docker loader
  URL:=https://www.docker.com/
  DEPENDS:= \
    +jsonfilter \
    +ca-certificates \
    +iptables \
    +iptables-mod-extra \
    +IPV6:ip6tables \
    +IPV6:kmod-ipt-nat6 \
    +kmod-ipt-nat \
    +kmod-ipt-physdev \
    +kmod-nf-ipvs \
    +kmod-veth
  USERID:=docker:docker
  MENU:=1
  PKGARCH:=all
endef

define Package/docker
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Docker Community Edition CLI
  URL:=https://www.docker.com/
  DEPENDS:=dockerd
  PKGARCH:=all
endef

define Package/docker/description
The CLI used in the Docker CE and Docker EE products.
endef

define Package/docker-compose
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Docker Compose
  URL:=https://github.com/docker/compose
  DEPENDS:=dockerd +docker
  PKGARCH:=all
endef

define Package/docker-compose/description
  Multi-container orchestration for Docker
endef


define Package/dockerd/conffiles
/etc/config/dockerd
/etc/config/dockeru
endef

define Package/dockerd/description
The Docker CE Engine loader.
endef

define Build/Configure
endef

define Build/Compile
endef

define Package/dockerd/install

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/dockerd.init $(1)/etc/init.d/dockerd

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/etc/config/dockerd $(1)/etc/config/dockerd

	$(INSTALL_DIR) $(1)/etc/hotplug.d/net
	$(INSTALL_DATA) ./files/etc/hotplug.d/net/br-netfilter \
		$(1)/etc/hotplug.d/net/10-docker-br-netfilter

	$(INSTALL_DIR) $(1)/lib/fstab.d
	$(INSTALL_DATA) ./files/lib/fstab.d/dockerd.sh $(1)/lib/fstab.d/dockerd.sh

	$(INSTALL_BIN) ./files/dockeru.init $(1)/etc/init.d/dockeru

	$(INSTALL_CONF) ./files/etc/config/dockeru $(1)/etc/config/dockeru

endef

define Package/dockerd/postinst
#!/bin/sh
[ -n "$$IPKG_INSTROOT" ] || {
	/etc/init.d/dockerd enable
	/etc/init.d/dockerd uciadd
	/etc/init.d/dockerd start
}
endef

define Package/dockerd/prerm
#!/bin/sh
[ -n "$$IPKG_INSTROOT" ] || {
	/etc/init.d/dockerd disable
	/etc/init.d/dockerd stop
	/etc/init.d/dockerd ucidel
}
endef

define Package/empty/install
# nothing to do
endef
Package/docker/install=$(Package/empty/install)
Package/docker-compose/install=$(Package/empty/install)

$(eval $(call BuildPackage,dockerd))
$(eval $(call BuildPackage,docker))
$(eval $(call BuildPackage,docker-compose))
