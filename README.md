# stm32mp157f-dk2
This repo contains the changes implemented to test out the stm32mp157f-dk2 board's features.

# STM32MP157F DK2 OpenAMP Example

This repository contains a CubeIDE project for the STM32MP157F-DK2 board, demonstrating
OpenAMP communication between the Cortex‑A7 (Linux) and Cortex-M4 (firmware) cores.

## ✨ Features
- Configured `.ioc` file for STM32MP157F DK2
- Virtual UART channels (`/dev/ttyRPMSG0`, `/dev/ttyRPMSG1`)
- Echo loop: Cortex‑M4 echoes back any message received from Cortex‑A7
- Ready to extend into command/response protocols or sensor streaming

## 🛠 Requirements
- STM32MP157F‑DK2 board
- STM32CubeIDE (tested with v1.x)
- OpenSTLinux distribution with remoteproc and rpmsg enabled

## 🚀 Usage
1. Build the project in CubeIDE.
2. Copy the generated ELF to your Linux board via powershell:
   ```bash
   scp Debug/MPU1_CM4.elf root@<board-ip>:/lib/firmware/
   ```
3. From step 3 onwards, execute it in the SSH/serial terminal connected via usb-eth or serial port on the board. 
Specify the firmware (copy your .elf to /lib/firmware/ first)
	```bash
	echo MPU1_CM4.elf > /sys/class/remoteproc/remoteproc0/firmware
	```
4. Start the process and check if it is running
    ```bash
    echo start > /sys/class/remoteproc/remoteproc0/state
    dmesg | grep -E "rpmsg|virtio|remoteproc|ttyRPMSG"
   ```
5. Send some strings and see the echo with another terminal
	(I used teraterm to open one via SSH and another via serial port)
	```bash
	echo "Ping from A7" > /dev/ttyRPMSG0
	```
	In the other terminal, execute the following command
	```bash
	cat /dev/ttyRPMSG0
	```
	

