# Review

Существенных замечаний к реализации не выявлено.

Проверено:

- `ipq40xx_cisco_ap1800s_defconfig` проходит Kconfig и собирает образ с entry point `0x84000000`.
- чистая сборка без заранее существующего `httpd/fsdata.c` генерирует его через зависимость `httpd/fs.o`;
- `switch_to_bridge()` имеет weak fallback, поэтому HTTPD не требует board-specific реализации;
- изменения `httpd/` пользователя сохранены в `stash@{0}` (`wip httpd changes`);
- итоговый RAM-образ совпадает с `/sdk1/uboot-cisco.bin` по SHA-256.
- Cisco target не содержит команды `saveenv`/`flashwrite`, persistent env backends или vendor default-env update-переменные;
- HTTP recovery оставлен только для диагностики, а его flash-update handler для Cisco target безусловно отказывает.

Ограничение: сборка этого старого fork требует `KCFLAGS=-Wno-error` из-за существующих предупреждений в сторонних подсистемах (`part_efi`, `log2`, QPIC). Это не связано с HTTPD и не скрывает ошибок линковки или DTB.
