# Review

Существенных замечаний нет. `SYS_TEXT_BASE` теперь является Kconfig-значением для Cisco-target и одинаково применяется линкером, C и assembler; embedded control-DTB входит в ELF `LOAD`-сегмент. Изменение изолировано условием `CISCO_AP1800S` и не меняет адреса остальных IPQ40xx targets.
