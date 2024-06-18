# padavan-4.4 #

This project is based on original rt-n56u with latest mtk 4.4.198 kernel, which is fetch from D-LINK GPL code.

- Features
  - Based on 4.4.198 Linux kernel
  - Support MT7621 based devices
  - Support MT7615D/MT7615N/MT7915D wireless chips
  - Support raeth and mt7621 hwnat with legency driver
  - Support qca shortcut-fe
  - Support IPv6 NAT based on netfilter
  - Support WireGuard integrated in kernel
  - Support fullcone NAT (by Chion82)
  - Support LED&GPIO control via sysfs


- Supported devices
  - CR660x
  - JCG-Q20
  - JCG-AC860M
  - JCG-836PRO
  - JCG-Y2
  - DIR-878
  - DIR-882
  - K2P
  - K2P-USB
  - NETGEAR-BZV
  - MR2600
  - MI-4
  - MI-R3G
  - MI-R3P
  - R2100
  - XY-C1

- Compilation step
  - Install dependencies
    ```sh
    # Debian/Ubuntu
    sudo apt install unzip libtool-bin curl cmake gperf gawk flex bison nano xxd \
        fakeroot kmod cpio git python3-docutils gettext automake autopoint \
        texinfo build-essential help2man pkg-config zlib1g-dev libgmp3-dev \
        libmpc-dev libmpfr-dev libncurses5-dev libltdl-dev wget libc-dev-bin

    # Archlinux/Manjaro
    sudo pacman -Syu --needed git base-devel cmake gperf ncurses libmpc \
            gmp python-docutils vim rpcsvc-proto fakeroot cpio help2man

    # Alpine
    sudo apk add make gcc g++ cpio curl wget nano xxd kmod \
        pkgconfig rpcgen fakeroot ncurses bash patch \
        bsd-compat-headers python2 python3 zlib-dev \
        automake gettext gettext-dev autoconf bison \
        flex coreutils cmake git libtool gawk sudo
    ```
  - Clone source code
    ```sh
    git clone https://github.com/meisreallyba/padavan-4.4.git
    ```
  - Prepare toolchain
    ```sh
    cd padavan-4.4/toolchain-mipsel

    # (Recommend) Download prebuilt toolchain for x86_64 or aarch64 host
    ./dl_toolchain.sh

    # or build toolchain with crosstool-ng
    # ./build_toolchain
    ```
  - Modify template file and start compiling
    ```sh
    cd padavan-4.4/trunk

    # (Optional) Modify template file
    # nano configs/templates/K2P.config

    # Start compiling
    fakeroot ./build_firmware_modify K2P

    # To build firmware for other devices, clean the tree after previous build
    ./clear_tree
    ```

- Manuals
  - Controlling GPIO and LEDs via sysfs
  - How to use NAND RWFS partition
  - How to use IPv6 NAT and fullcone NAT
  - How to add new device support with device tree

 
- 1.基于linux-3.4.x内核版本。2023-6-28同步桌面
- 2.固件默认设置：wifi名称"Finer_%s"及"Finer5G_%s"，密码:1234567890；管理地址192.168.10.1，管理账号:amin,密码:admin。
- 3.常用文件目录：
  -(1)版本信息trunk/versions.inc；机型文件trunk/configs/（boards为配置文件夹，templates为Config文件）；
  - (2)固件默认配置信息trunk/user/shared/defaults.h； 管理页面页脚信息（2个地方）trunk/user/www/Makefile；还有一个地方。
  - (3)图标文件夹trunk/user/www/n56u_ribbon_fixed/bootstrap/img（asus_logo.png为logo）； 
  - (4)背景设置trunk/user/www/n56u_ribbon_fixed/bootstrap/css/main.css；中文适配信息trunk/user/www/dict/CN.dict； 
  - (5)默认信息修改trunk/user/shared/defaults.h。其中#define DEF_WLAN_2G_CC与DEF_WLAN_5G_CC必须慎重修改。选择"GB"-（区域代码:Europe (channels 36,40,44,48)），会适合新旧机器网卡，速度均衡；选择“US”与“CN”频道多，对于新机器设备如IPhone等网速更快；本人选择较老款的MacBook Pro，选择“GB”速度快！选择“US”或“CN”速度将至10%（内核4.4版本使用5G-“US”）。此项固件生成刷机后也可在路由器后台修改。
  - (6)插件安装配置文档trunk/build_firmware_modify。一般在编译自动化流文本中重新自定义安装插件编译的要求github/workflows。 4.小米路由器R3Gv2版本配置与小米R4A（旧版）一致，固件可以通用。
