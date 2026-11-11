# Flash Commander Wi-Fi on Windows

## Check your board

This kit targets the **Guition JC4880P443C_I_W/Y**, a 4.3-inch, 480 × 800 touch display with an ESP32-P4, ST7701S LCD controller and 16 MB flash. The Commander build is for the early ESP32-P4 silicon range accepted by the image, v0.1 through v1.99. Use the exact board and verify its chip revision. Similar-looking Guition displays can have different panels, flash layouts or ports. [Guition board specifications](https://www.guition.com/icms/upload/fb081940d6fc11f09850077a33e1404f/file/productmanager-productfile/5dfbe9a7c0fc44869270528bd2411b1e/Directory/JC4880P443C_I_W%20Specifications-EN-V1.0_1776241869964.pdf)

| Port | Use |
|---|---|
| USB2 | Windows flashing in bootloader mode. In Commander, this port hosts the OneROM. |
| USB3 | Power during Wi-Fi operation or connect Windows for SD Card Access. |

Use a data-capable USB cable and Windows x64. Disconnect OneROM and any vehicle wiring for flashing. The merged image replaces the full 16 MB of P4 flash, including saved settings. Back up the original image first if you want a way back. This kit updates the P4 and leaves the separate wireless coprocessor firmware alone.

## Enter the bootloader

1. With the private Commander Wi-Fi kit available, use **Extract All** on its ZIP. Keep the launcher, firmware and **tools** folder together.
2. Open **Device Manager → Ports (COM & LPT)** so you can see which port appears.
3. Unplug the USB and external power cables from the P4. Hold the board’s **BOOT** button while reconnecting a Windows data cable to **USB2**. On the owner’s tested unit this was the leftmost button. Use the board marking or vendor instructions on other revisions.
4. Release BOOT after Windows shows a new COM port. The LCD can stay blank in the bootloader. Write down the COM number. If several ports appear, compare the list with the board unplugged and re-enter bootloader mode.

## Optional backup

Open PowerShell in the extracted folder. Replace `COM7` with your actual bootloader port. Pick a new filename and store the backup privately. Keep the P4 connected until the command completes.

```powershell
.\tools\esptool.exe --chip esp32p4 --port COM7 --baud 115200 --before no_reset --after no_reset read_flash 0x0 0x1000000 p4-original-backup.bin
```

## Flash and check the result

1. Double-click **Flash Commander.cmd**. Enter the P4’s COM port. The launcher checks the firmware checksum and asks esptool to identify the chip, revision and flash size before offering to write.
2. Review the details. Type **FLASH** only for the intended board. The launcher writes the **merged** image at **0x0** and esptool checks the data it wrote. Keep the cable attached until the result is shown.
3. After success, release BOOT if you are still holding it. If the P4 stays in the bootloader, unplug and reconnect its power with BOOT released.
4. Look for the **Rapid Fire Commander** fade-in followed by the **Commander** dashboard. Verify touch response and check **Settings → Brightness**. Reconnect OneROM after the first boot check.

## Troubleshooting

| Symptom | What to check |
|---|---|
| No COM port | Use a known data cable directly to the computer. Check USB2 and repeat the BOOT sequence. Look in Device Manager for the new device. |
| Failed to connect or port busy | Close TunerPro, serial monitors and other flash tools. Confirm the COM number and repeat the BOOT sequence. |
| The launcher rejects the chip, revision or flash size | Compare the board label and chip identification with the compatibility section above. |
| Flash verification fails | Keep OneROM unplugged. Check cable and power, re-enter bootloader mode and retry the same kit. |
| Screen stays blank after success | Boot normally with BOOT released. If it stays blank, re-enter the bootloader, check the exact board model, and use the original backup for recovery if needed. |
| COM port disappears during normal boot | USB2 changes roles in Commander. Select **TunerPro bridge**, connect the Windows app over Wi-Fi, then use the mapped COM port in TunerPro RT. |

The kit uses the [official Windows x64 esptool v4.9.1 release](https://github.com/espressif/esptool/releases/tag/v4.9.1).
