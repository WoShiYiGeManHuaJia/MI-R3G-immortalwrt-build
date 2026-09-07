include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=rkp-ipid
PKG_RELEASE:=2

include $(INCLUDE_DIR)/package.mk

define KernelPackage/rkp-ipid
	SUBMENU:=Other modules
	TITLE:=rkp-ipid (IP-ID detection evasion, patched for new kernels)
	FILES:=$(PKG_BUILD_DIR)/rkp-ipid.ko
	AUTOLOAD:=$(call AutoLoad, 99, rkp-ipid)
	KCONFIG:=
endef

# 源码在 src/ 子目录，直接复制进构建目录（老包没有 PKG_SOURCE 打包规范）
define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) $(CURDIR)/src/* $(PKG_BUILD_DIR)/
endef

EXTRA_KCONFIG:= \
	CONFIG_RKP_IPID=m

EXTRA_CFLAGS:= \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=m,%,$(filter %=m,$(EXTRA_KCONFIG)))) \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=y,%,$(filter %=y,$(EXTRA_KCONFIG)))) \
	-DVERSION=$(PKG_RELEASE)

MAKE_OPTS:=$(KERNEL_MAKE_FLAGS) \
	M="$(PKG_BUILD_DIR)" \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
	$(EXTRA_KCONFIG)

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" $(MAKE_OPTS) modules
endef

$(eval $(call KernelPackage,rkp-ipid))
