# RZ Build Scripts

* These scripts allow you to build BSP software components outside of the Yocto build environment.
* The following boards are supported:

<table cellpadding=2 border=1 style="border:1px solid black; border-collapse: collapse;">
<tr><th>Series</th><th>SoC</th><th>Board</th><th>BSP</th></tr>
<tr>
    <td rowspan=2>RZ/G3 Series</td>
</tr>
    <tr><td>RZ/G3S</td><td>smarc-rzg3s</td><td>VLP 3.x</td></tr>
<tr>
    <td rowspan=8>RZ/G2 Series</td>
</tr>
    <tr><td>RZ/G2H</td><td>hihope-rzg2h</td><td>VLP 3.x</td></tr>
    <tr><td>RZ/G2M</td><td>hihope-rzg2m</td><td>VLP 3.x</td></tr>
    <tr><td>RZ/G2N</td><td>hihope-rzg2n</td><td>VLP 3.x</td></tr>
    <tr><td>RZ/G2E</td><td>ek874</td><td>VLP 3.x</td></tr>
    <tr><td>RZ/G2L</td><td>smarc-rzg2l</td><td>VLP 3.x</td></tr>
    <tr><td>RZ/G2LC</td><td>smarc-rzg2lc</td><td>VLP 3.x</td></tr>
    <tr><td>RZ/G2UL</td><td>smarc-rzg2ul</td><td>VLP 3.x</td></tr>
<tr>
    <td rowspan=3>RZ/V Series</td>
</tr>
    <tr><td>RZ/V2H</td><td>rzv2h-evk-ver1</td><td> </td></tr>
    <tr><td>RZ/V2L</td><td>smarc-rzv2l</td><td></td></tr> 
<tr>
    <td rowspan=2>RZ/T Series</td></td>
</tr>
    <tr><td>RZ/V2L</td><td>dev-rzt2h</td><td> </td></tr>
</table>

## Repository Installs

* These scripts will not download any source code repositories. You must do that manually.
* Please refer to file **Repository Installs.txt** to find copy/paste commands to get the source code for each BSP software component.
* Note that for some repositories, especially the kernel repositories, you may need additional patches that are not in the public github repositories. Those patches are only distributed in the BSP pacakge that is downloaded from renesas.com.

## Toolchain Installs

* You can use the Yocto SDK Toolchain from the Renesas BSP.
* You can also use an external toolchain such as Linaro or ARM.
* Please refer to file **Toolchain Installs.txt** to find copy/paste commands to download and install pre-built Toolchains.

## Source Code Directory Locations and Names

* This script assumes that your source code repositories are kept in a directory under this one and they use the **same directory names** as below.
* To use a different directly name, you can manually edit the **board.ini** file.

* For example:
<pre>
└── rz_build_scripts/
    ├── mbedtls/                   <<<<<< you add
    ├── renesas-u-boot-cip/        <<<<<< you add
    ├── rzg2_flash_writer/         <<<<<< you add
    ├── rzg_trusted-firmware-a/    <<<<<< you add
    ├── rz_linux-cip/              <<<<<< you add
    ├── build.sh
    ├── build_xxxx.sh
    ├── build_xxxx.sh
    └── README.md
</pre>

## Board Settings File (board.ini)

* All the configuration settings for your board will be saved in a **board.ini** file.
* This file will be automatically created for you when you use the setup command **./build.sh s**
* You should not need to modify any of the build_xxx.sh files.

## Output Directory

* After each build, the files you need will be copied to an output directory named output_xxxx where xxxx is the name of your board.

<pre>
└── rz_build_scripts
    ├── rzg_trusted-firmware-a/
    ├── renesas-u-boot-cip/
    ├── output_xxxx/               <<<<<< this will be created
    ├── build.sh
    └── README.md
</pre>

## Getting Started

1) Install a toolchain as explained in the **Toolchain Installs.txt** document.

2) Download (clone) the source code repositories as explained in the **Repository Installs.txt** document and apply any patches that are needed.

3) Use command 's' first to **select** your board and toolchain.
<pre>
$ ./build.sh s
</pre>

4) Run the **build.sh** script with no arguments to get a list of command options. **Do not run the other build_xxx.sh file directly.** Only call build.sh.
Example:
<pre>
$ ./build.sh s             # Select your target board
$ ./build.sh               # Show a list of command options
$ ./build.sh f             # Build flash writer
$ ./build.sh u             # Build u-boot
$ ./build.sh t             # Build trusted firmware
$ ./build.sh k             # Build Linux kernel
</pre>

## Using a Custom Board

These scripts can be used to build images for non-Renesas boards.

The procedure is as follows:

1) Use the command "./build.sh s" and select a Renesas Evaluation board with the same device as your custom board. This will create a board.ini file that you can then customize.

2) Manually edit the file **board.ini** and make the following changes:

**MACHINE=xxxxx**

* Match your board name (MACHINE) that you use for your Yocto build configuration

**OUT_DIR=output\_xxxx**

* This is the directory where all the output files from each build are copied to.

**FW_BOARD=xxxx**

* Flash Writer does not use the MACHINE name for building. Instead, it uses board BOARD=xxxx.
* Make this setting match what you want to pass as BOARD=xxxx on the build command line

3) Create Configuration Files

* Please note that since you have changed the MACHINE setting to xxxx, you will need to add the following files:

**u-boot:**

* rzg\_trusted-firmware-a/configs/xxx_defconfig
* You will find examples of the Renesas  boards in that configs directory

**Trusted Firmware-A:**

* You need a directory that matches the MACHINE name under rzg_trusted-firmware-a/plat/renesas/rz/board/
* The build system will look for the file rz_board.mk in that directory.
* Example: rzg\_trusted-firmware-a/plat/renesas/rz/board/**xxxx**/rz_board.mk





