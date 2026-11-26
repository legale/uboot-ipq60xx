obj-y := cmd_bootqca.o
obj-y += cmd_blowsecfuse.o
obj-y += cmd_sec_auth.o
obj-y += cmd_exectzt.o
obj-$(CONFIG_SMP_CMD_SUPPORT) += cmd_runmulticore.o
obj-y += fdt_info.o
obj-y += board_init.o
ifeq ($(CONFIG_ENV_IS_NOWHERE)$(CONFIG_ENV_IS_IN_UBI),)
obj-y += env.o
endif
obj-y += fdt_fixup.o
obj-y += ethaddr.o
obj-$(CONFIG_QCA_APPSBL_DLOAD) += crashdump.o
