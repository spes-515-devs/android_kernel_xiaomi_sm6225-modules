KERNEL_SRC ?= /lib/modules/$(shell uname -r)/build

# The Make variable $(M) must point to the directory that contains the module
# source code (which includes this Makefile). It can either be an absolute or a
# relative path. If it is a relative path, then it must be relative to the
# kernel source directory (KERNEL_SRC). An absolute path can be obtained very
# easily through $(shell pwd). Generating a path relative to KERNEL_SRC is
# difficult and we accept some outside help by letting the caller override the
# variable $(M). Allowing a relative path for $(M) enables us to have the build
# system put output/object files (.o, .ko.) into a directory different from the
# module source directory.
M ?= $(shell pwd)

ifeq ($(WLAN_ROOT),)
KBUILD_OPTIONS += \
    WLAN_ROOT=$(KERNEL_SRC)/$(M) \
    WLAN_PLATFORM_ROOT=$(KERNEL_SRC)/$(M)/../platform \
    WLAN_COMMON_ROOT=cmn \
    WLAN_COMMON_INC=$(KERNEL_SRC)/$(M)/cmn \
    WLAN_FW_API=$(KERNEL_SRC)/$(M)/../fw-api \
    BOARD_PLATFORM=bengal \
    CONFIG_QCA_CLD_WLAN=m \
    WLAN_PROFILE=default \
    CONFIG_ICNSS2=m \
    CONFIG_ICNSS2_QMI=y \
    CONFIG_CNSS_OUT_OF_TREE=y \
    CONFIG_CNSS_QMI_SVC=m \
    CONFIG_CNSS_GENL=m \
    CONFIG_WCNSS_MEM_PRE_ALLOC=m \
    CONFIG_CNSS_UTILS=m \
    MODNAME=wlan \
    KERNEL_SUPPORTS_NESTED_COMPOSITES=n
endif

KBUILD_OPTIONS += KBUILD_EXTRA_SYMBOLS=$(OUT_DIR)/../sm6225-modules/qcom/opensource/wlan/platform/Module.symvers

all:
	$(MAKE) -C $(KERNEL_SRC) M=$(M) modules $(KBUILD_OPTIONS)

modules_install:
	$(MAKE) INSTALL_MOD_STRIP=1 M=$(M) -C $(KERNEL_SRC) modules_install

clean:
	$(MAKE) -C $(KERNEL_SRC) M=$(M) clean $(KBUILD_OPTIONS)
