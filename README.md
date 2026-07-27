# xiaozhi-esp32-firmware

小智 ESP32-S3（**Korvo2-V3**）韌體 build 產物備份。含雙麥（DUAL_MIC）客製。

> bin 檔不放在 git 歷史裡，一律掛在 **Releases**。

## 版本

| 版本 | 晶片/板型 | ESP-IDF | 說明 |
|---|---|---|---|
| 2.2.8 | esp32s3 / Korvo2-V3 | v5.5.3 | 誤喚醒門檻 25、雙麥客製；現場部署版 |

## 燒錄

Linux 埠通常是 `/dev/ttyACM0`（查 `ls /dev/ttyACM* /dev/ttyUSB*`）。

**單檔（最簡單）**
```
esptool --chip esp32s3 -p /dev/ttyACM0 -b 460800 write_flash 0x0 merged-flash.bin
```

**分檔**
```
esptool --chip esp32s3 -p /dev/ttyACM0 -b 460800 --before default_reset --after hard_reset \
  write_flash --flash_mode dio --flash_size 16MB --flash_freq 80m \
  0x0 bootloader.bin 0x8000 partition-table.bin 0xd000 ota_data_initial.bin \
  0x20000 xiaozhi.bin 0x800000 generated_assets.bin
```

沒裝 esptool 可用 `uvx --from esptool esptool ...`。

## 完整性 (2.2.8)

- merged-flash.bin sha256: `00f9d6b88f8d4ffcbd58098955856536273225173ae805df3e4967f600e8d068`
- xiaozhi.bin sha256: `e894dea06cd9d3b7c4e740a24643952ed75e3216e3140b561bf5474c18d99350`
