# Onboarding Project - Introduction to Embedded 1

## Overview

The end goal of this project is to write firmware for an embedded system in order to blink an LED. The codebase will be set up for you, and all you have to do is write the actual blink function. This is mostly just familiarizing yourself with the necessary tools and the general layout of an STM32 codebase.

## Background info

For many of you, this is your first time hearing about an embedded system, and that is OK! An embedded system is simply a small computer designed to perform a specific task and is often times integrated into larger system. This differs from a regular computer, which is more general-purpose. Embedded systems are usually less powerful and have limited resources, but they are much smaller and cheaper. They can be found in cars, medical equipment—or, in our case, a robot!

Some terms to understand:

- **Hardware Abstraction Layer (HAL)**: A software layer that allows your application to directly interact with the hardware of the microcontroller.
- **General Purpose Input/Output (GPIO)**: Pins on a microcontroller that can be configured as either inputs or outputs. They can be set to high (on) or low (off).

## Prerequisites

- A laptop, (either MacOS or Windows) with a usb port.
- Basic knowledge of C.

## [!] Start Here

### Part 1: Download Tools

In order to complete this project you will need to download the following softwares:

- **VSCode** - A text editor with extensions. Download the appropriate version from [here](https://code.visualstudio.com/download)
- **Package Manager**
  - Windows users should install [MSYS2](https://www.msys2.org/)
  - MacOS users should install [Homebrew](https://brew.sh/)
- **OpenOCD** - Used for flashing and debugging. Install it using the appropriate package manager by running a command in your terminal.
  - Windows: `pacman -S mingw-w64-x86_64-openocd`
  - MacOS: `brew install openocd`
- **Arm-Embedded Toolchain** - A collection of tools used to develop software for ARM Cortex MCUs. Install using the same process as OpenOCD.
  - Windows: `pacman -S mingw-w64-x86_64-arm-none-eabi-gcc`
  - MacOS: `brew install --cask gcc-arm-embedded`
- **Make** - Automates the build process using a Makefile.
  - Windows: `pacman -S mingw-w64-x86_64-make`
  - MacOS: You should already have it installed, but if you don’t, then use `brew install make`
- **ST-Link** - Flashes the firmware to STM32 microcontrollers and enables debugging.
  - Windows: `pacman -S mingw-w64-x86_64-stlink`
  - MacOS: `brew install stlink`.

**Important!** - Add your tools to PATH variable so that they can be accessed from anywhere in the terminal.
- Windows: Follow the instructions from [this article](https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/) to add `C:\msys64\mingw64\bin` to PATH.
- MacOS: Use a text editor to add the following line to your shell’s configuration file (e.g., .bashrc, .zshrc): `export PATH="/opt/homebrew/bin:$PATH"`.
- Note that the install location may be different for you, update your enviroment variable as necessary.

Don't hesitate to ask for help here, as this is the most confusing step.

### Part 2: Download the Codebase

1. In order to get the codebase onto your local machine, simply clone the repository. Run the following command in your terminal.

```
git clone https://github.com/PurdueRM/Onboarding-Project-1.git
```

2. Now that the codebase is on your local machine, open it using VSCode, installed from Part 1.
3. Open the `main.c` file located in `src/`.
4. You should see a lot of stuff but you can ignore everything for now, and just locate `main()`.

### Part 3: Your First Function!

1. In the `main()` function, find the infinite while loop (`while(1)`). You will be writing your code in here.
2. Paste the following line of code in the while loop under the comment that says "USER CODE BEGIN 3". This line of code uses a HAL function to set GPIO pin B3 to a high state (1), turning on the green LED connected to it.

```
HAL_GPIO_WritePin(GPIOB, GPIO_PIN_3, GPIO_PIN_SET);
```

3. Next, in order to blink the LED, we must delay for a certain amount of time before turning it off. For this we will use the following line of code to pause the program for 1000 milliseconds (1 second).

```
HAL_Delay(1000);
```

4. Add the following line of code to turn off the green LED by setting GPIO pin B3 to a low state (0):

```
HAL_GPIO_WritePin(GPIOB, GPIO_PIN_3, GPIO_PIN_RESET);
```

5. Write a line of code to delay for another second before the loop restarts.
   If you completed this part correctly, you should have written four lines of code total.

### Part 4: Compile the Code

1. Open the VSCode command palette using `CMD/CTRL + SHIFT + P`
2. Search for the "Run Tasks" command by typing it into the command bar and hit enter to select it.
3. From here you should see three options, "build", "clean", and "build and flash".
4. Select "build" and hit enter. This should start to compile your project and any compile errors will be shown.

### Part 5: Flash the Code

Once you make it to this part, this is where you will need to come to a meeting so that we can give you a board and help you to flash it and see your code in action.

1. Connect the board to your computer using USB. The board should light up when it receives power.
2. Open the VSCode command palette and run the "build and flash" command. You should see the big LED start blinking briefly.
3. Check if the small green LED is blinking slowly. If it is, congrats, you're finished!
