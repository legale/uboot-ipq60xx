# Cisco AIR-AP1800S: рабочий RAM-chainload U-Boot

Рабочий raw U-Boot запускается из штатного Cisco BTLDR командой `go` по адресу `0x84000000`. UART сохраняет 9600, DRAM определяется как 256 MiB (254 MiB usable), сеть работает через внешний AR8033.

Последний проверенный образ:

- размер: `452720` байт
- SHA-256: `0c8469ded18f8ce68c786b20b7ce01a61e8c373c1740fc96811fd74ff359b05e`
- TFTP: `/srv/tftp/uboot-cisco.bin`, `/srv/tftp/uboot-cisco_ap1800s.bin`
- копии: `/sdk1/uboot-cisco.bin`, `/sdk1/uboot-cisco_ap1800s.bin`
- удалённые копии: `172.16.130.13:/tmp/uboot-cisco.bin` и `/tmp/uboot-cisco_ap1800s.bin`

## Запуск

```text
tftpboot 0x84000000 172.16.130.13:uboot-cisco_ap1800s.bin
go 0x84000000
```

После prompt:

```text
env set ipaddr 172.16.130.254
env set serverip 172.16.130.13
ping 172.16.130.13
tftpboot 0x85000000 172.16.130.13:<image>
```

## Аппаратные факты

- BTLDR сообщает `Board type: AP1800S`, `machid: 8010001`.
- RAM платы: 256 MiB (BTLDR показывает 254 MiB usable); SMEM-таблица ошибочно сообщает 1 GiB.
- PHY: `AR8033`, MDIO address 0, интерфейс RGMII.
- MAC из ART: `48:8b:0a:a0:53:a0`.
- Профиль DK04 с QCA8075/PSGMII для этой платы неверен.

## Основные решения

- `CONFIG_SYS_TEXT_BASE=0x84000000`, запуск только raw BIN через `go`.
- DTS: `ddr_size=<256>`, UART `bit_rate=<0x99>`, `edma_cfg phy=<7>` (`RGMII`).
- Cisco `dram_init()` игнорирует stale SMEM RAM table и использует DTS.
- Environment RAM-only; автоматическая запись env, APPSBL и SPI/NAND отключена.
- Автоматические Ethernet init и `bootipq` до prompt отключены; сеть поднимается при `ping`/`tftpboot`.
- Используется `ipq_qca8033_phy_init(ops, 0)`, проверяется только PHY0.
- PSGMII self-test отключён; ESS настроен для одиночного AR8033 через RGMII.
- Для Cisco объявляется один MAC, поэтому ложный `eth1 invalid` устранён.

## Сборка

```bash
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- HOST_LOADLIBES=-lbsd ipq40xx_cisco_ap1800s_defconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- HOST_LOADLIBES=-lbsd KCFLAGS=-Wno-error -j"$(nproc)" u-boot.bin
```

Отдельный ELF не нужен: в штатном Cisco BTLDR отсутствует `bootelf`.
