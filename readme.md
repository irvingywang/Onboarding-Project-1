# Onboarding Project - Introduction to Embedded 1

## Overview

The end goal of this project is to write firmware for an embedded system in order to blink an LED. The codebase will be set up for you, and all you have to do is write the actual blink function. This is mostly just familiarizing yourself with the necessary tools and the general layout of an STM32 codebase.

## Background info

For many of you, this is your first time hearing about an embedded system, and that is OK! An embedded system is simply a small computer designed to perform a specific task and is often times integrated into a larger system. This differs from a regular computer, which is more general-purpose. Embedded systems are usually less powerful and have limited resources, but they are much smaller and cheaper. They can be found in cars, medical equipment—or, in our case, a robot!

Some terms to understand:

- **Hardware Abstraction Layer (HAL)**: A software layer that allows your application to directly interact with the hardware of the microcontroller.
- **General Purpose Input/Output (GPIO)**: Pins on a microcontroller that can be configured as either inputs or outputs. They can be set to high (on) or low (off).

## Prerequisites

- A laptop (MacOS or Windows) with a USB port.
- Basic knowledge of C.
- **Windows users only:** Windows 10 version 2004+ (build 19041+) or Windows 11, which is required for WSL2.

---

## [!] Start Here

## Part 1: Download Tools

### MacOS

Install the following via [Homebrew](https://brew.sh):

- **VSCode** — text editor with extensions. Download from [here](https://code.visualstudio.com/).
- **OpenOCD** — used for flashing and debugging.
  ```
  brew install openocd
  ```
- **Arm-Embedded Toolchain** — collection of tools for developing on ARM Cortex MCUs.
  ```
  brew install --cask gcc-arm-embedded
  ```
- **Make**
  ```
  brew install make
  ```
- **ST-Link** — flashes firmware to STM32 microcontrollers and enables debugging.
  ```
  brew install stlink
  ```

Add Homebrew's bin to your `PATH` by adding this line to your shell config (`.bashrc`, `.zshrc`):
```
export PATH="/opt/homebrew/bin:$PATH"
```
(Your install location may differ — update as necessary.)

### Windows (via WSL)

We use **WSL (Windows Subsystem for Linux)** rather than MSYS2. This lets you use the same Linux-based toolchain and commands as MacOS users, which is more consistent and much less prone to PATH issues.

**1. Install WSL2 and Ubuntu**

Open PowerShell **as Administrator** and run:
```
wsl --install
```
This installs WSL2 with Ubuntu as the default distro. Restart your computer when prompted, then finish setup by creating a Linux username/password when Ubuntu launches.

If you already have WSL installed but not Ubuntu:
```
wsl --install -d Ubuntu
```

**2. Install the toolchain inside WSL (Ubuntu)**

Open the Ubuntu terminal (search "Ubuntu" in the Start menu) and run:
```
sudo apt update
sudo apt install -y build-essential gcc-arm-none-eabi openocd stlink-tools git make
```
This one step replaces the separate package-manager, OpenOCD, ARM toolchain, Make, and ST-Link installs that MSYS2 required — `apt` handles PATH setup automatically, so there's no manual PATH editing needed on Windows.

**3. Install VS Code + the WSL extension**

- Install [VS Code](https://code.visualstudio.com/) normally on Windows (not inside WSL).
- Open VS Code, go to Extensions, and install **"WSL"** (by Microsoft).
- You'll open and build the project through VS Code's WSL-connected window (bottom-left green `><` icon will say "WSL: Ubuntu").

**4. Install `usbipd-win` (for flashing over USB)**

WSL2 doesn't see USB devices by default. To flash your board via ST-Link, you need to pass the USB device through to WSL.

In PowerShell **as Administrator**:
```
winget install usbipd
```
You'll use this each time you plug in your ST-Link/board — instructions are in Part 5.

---

## Part 2: Download the Codebase

**MacOS:** Clone the repository anywhere convenient:
```
git clone https://github.com/PurdueRM/Onboarding-Project-1.git
```

**Windows (WSL):** Clone the repository **inside your WSL Linux filesystem**, not on the Windows `C:\` drive (i.e., not under `/mnt/c/...`). Cloning inside WSL's native filesystem is significantly faster to build and avoids permission/line-ending issues:
```
cd ~
git clone https://github.com/PurdueRM/Onboarding-Project-1.git
```

Then open it with VS Code directly from the WSL terminal, which automatically launches VS Code connected to WSL:
```
cd Onboarding-Project-1
code .
```

Open the `main.c` file located in `Core/Src/`. You should see a lot of stuff but you can ignore everything for now, and just locate `main()`.

## Part 3: Your First Function!

1. In the `main()` function, find the infinite while loop (`while(1)`). You will be writing your code in here.
2. Paste the following line of code in the while loop. This line of code uses a HAL function to set GPIO pin B3 to a high state (1), turning on the green LED connected to it.
   ```c
   HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET);
   ```
3. Next, in order to blink the LED, we must delay for a certain amount of time before turning it off. For this we will use the following line of code to pause the program for 1000 milliseconds (1 second):
   ```c
   HAL_Delay(1000);
   ```
4. Add the following line of code to turn off the green LED by setting GPIO pin B3 to a low state (0):
   ```c
   HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET);
   ```
5. Write a line of code to delay for another second before the loop restarts. If you completed this part correctly, you should have written four lines of code total.

## Part 4: Compile the Code

1. Open the VSCode command palette using `CMD/CTRL + SHIFT + P`.
2. Search for the **"Run Tasks"** command and hit enter to select it.
3. From here you should see options including **"Build"**, **"Clean"**, and **"Build and Flash"**.
4. Select **"Build"** and hit enter. This should start to compile your project and any compile errors will be shown.

> **Note for Windows/WSL users:** Make sure your VS Code window is connected to WSL (check the green `><WSL: Ubuntu` indicator in the bottom-left corner) before running tasks — otherwise VS Code will try to run `make` on native Windows, where it isn't installed.

## Part 5: Flash the Code

Once you make it to this part, this is where you will need to come to a meeting so that we can give you a board and help you to flash it and see your code in action.

**MacOS:**
1. Connect the board to your computer using USB. The board should light up when it receives power.
2. Open the VSCode command palette and run the **"Build and Flash"** command. You should see the big LED start blinking briefly.
3. Check if the small green LED is blinking slowly. If it is, congrats, you're finished!

**Windows (WSL):**
1. Connect the board to your computer using USB. The board should light up when it receives power.
2. Attach the USB device to WSL. In PowerShell **as Administrator**, list connected devices:
   ```
   usbipd list
   ```
   Find your ST-Link/board in the list and note its `BUSID` (e.g., `1-4`), then bind and attach it:
   ```
   usbipd bind --busid <BUSID>
   usbipd attach --wsl --busid <BUSID>
   ```
   You'll need to run `usbipd attach` again each time you unplug/replug the board or restart your machine (bind only needs to happen once).
3. Back in VS Code (connected to WSL), open the command palette and run **"Build and Flash"**. You should see the big LED start blinking briefly.
4. Check if the small green LED is blinking slowly. If it is, congrats, you're finished!

If `usbipd` doesn't detect your device or the flash fails, double check the board shows up under `wsl.exe` with:
```
lsusb
```
(run inside the WSL Ubuntu terminal) — if it's missing, re-run the `usbipd attach` step.
