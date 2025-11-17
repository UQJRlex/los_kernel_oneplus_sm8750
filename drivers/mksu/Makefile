kernelsu-objs := ksu.o
kernelsu-objs += allowlist.o
kernelsu-objs += app_profile.o
kernelsu-objs += apk_sign.o
kernelsu-objs += sucompat.o
kernelsu-objs += syscall_hook_manager.o
kernelsu-objs += throne_tracker.o
kernelsu-objs += pkg_observer.o
kernelsu-objs += setuid_hook.o
kernelsu-objs += kernel_umount.o
kernelsu-objs += supercalls.o
kernelsu-objs += feature.o
kernelsu-objs += ksud.o
kernelsu-objs += embed_ksud.o
kernelsu-objs += seccomp_cache.o
kernelsu-objs += file_wrapper.o

kernelsu-objs += selinux/selinux.o
kernelsu-objs += selinux/sepolicy.o
kernelsu-objs += selinux/rules.o
ccflags-y += -I$(srctree)/security/selinux -I$(srctree)/security/selinux/include
ccflags-y += -I$(objtree)/security/selinux -include $(srctree)/include/uapi/asm-generic/errno.h

obj-$(CONFIG_KSU) += kernelsu.o

KDIR := $(KDIR)
MDIR := $(realpath $(dir $(abspath $(lastword $(MAKEFILE_LIST)))))

$(info -- KDIR: $(KDIR))
$(info -- MDIR: $(MDIR))

ccflags-y += -DKSU_VERSION=22138

ifndef KSU_EXPECTED_SIZE
KSU_EXPECTED_SIZE := 384
endif

ifndef KSU_EXPECTED_HASH
KSU_EXPECTED_HASH := 7e0c6d7278a3bb8e364e0fcba95afaf3666cf5ff3c245a3b63c8833bd0445cc4
endif

ifdef KSU_MANAGER_PACKAGE
ccflags-y += -DKSU_MANAGER_PACKAGE=\"$(KSU_MANAGER_PACKAGE)\"
$(info -- KernelSU Manager package name: $(KSU_MANAGER_PACKAGE))
endif

$(info -- KernelSU Manager signature size: $(KSU_EXPECTED_SIZE))
$(info -- KernelSU Manager signature hash: $(KSU_EXPECTED_HASH))

ccflags-y += -DEXPECTED_SIZE=$(KSU_EXPECTED_SIZE)
ccflags-y += -DEXPECTED_HASH=\"$(KSU_EXPECTED_HASH)\"

ccflags-y += -Wno-strict-prototypes -Wno-int-conversion -Wno-gcc-compat -Wno-missing-prototypes
ccflags-y += -Wno-declaration-after-statement -Wno-unused-function

all:
	make -C $(KDIR) M=$(MDIR) modules
compdb:
	python3 $(MDIR)/.vscode/generate_compdb.py -O $(KDIR) $(MDIR)
clean:
	make -C $(KDIR) M=$(MDIR) clean

# Keep a new line here!! Because someone may append config
