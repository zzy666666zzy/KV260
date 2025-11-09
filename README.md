#  KV260 Build PetaLinux for KV260 Development Kit

Refined workflow based on official AMD/Xilinx guide

# Download Required Files

PetaLinux 2021.1:
https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools/archive.html

BSP (SOM StarterKit): xilinx-k26-starterkit-v2021.1-final.bsp
https://account.amd.com/en/forms/downloads/xef.html?filename=xilinx-k26-starterkit-v2021.1-final.bsp

# Install Dependencies
Install all required dependencies:
```
sudo apt-get install iproute2 gawk python3 python build-essential gcc git make net-tools \
libncurses5-dev tftpd zlib1g-dev libssl-dev flex bison libselinux1 gnupg wget git-core \
diffstat chrpath socat xterm autoconf libtool tar unzip texinfo zlib1g-dev gcc-multilib \
automake zlib1g:i386 screen pax gzip cpio python3-pip python3-pexpect xz-utils debianutils \
iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint3
```

# Install PetaLinux
1. Make installer executable
```
chmod +x ~/Downloads/petalinux-v2021.1-final-installer.run
```
2. Create a working directory
```
mkdir -p ~/petalinux/2021.1
```
3. Run installer
```
~/Downloads/petalinux-v2021.1-final-installer.run -d ~/petalinux/2021.1
```

# Configure Shell Environment
Ensure bash is used instead of dash
sudo dpkg-reconfigure dash
Select "No" when prompted: “Use dash as the default system shell?”

Source PetaLinux environment
```
cd ~/petalinux/2021.1
source settings.sh
```

# Create and Build Project
1. Create new project from BSP and build project
```
petalinux-create -t project -s xilinx-k26-starterkit-v2021.1-final.bsp
cd xilinx-k26-starterkit-2021.1
petalinux-build
```
Example of a common build error:
```
ERROR: Nothing RPROVIDES 'misc-config'
(but your_path/kv260/petalinux/xilinx-k26-starterkit-2021.1/components/yocto/layers/meta-petalinux/recipes-core/images/petalinux-initramfs-image.bb
RDEPENDS on or otherwise requires it)

NOTE: Runtime target 'misc-config' is unbuildable, removing...
Missing or unbuildable dependency chain was: ['misc-config']

ERROR: Required build target 'petalinux-image-minimal' has no buildable providers.
Missing or unbuildable dependency chain was:
['petalinux-image-minimal', 'u-boot-zynq-uenv', 'virtual/kernel', 'petalinux-initramfs-image', 'misc-config']

Summary: There were 2 ERROR messages shown, returning a non-zero exit code.
```

Refer to the official AMD/Xilinx solution:
https://adaptivesupport.amd.com/s/feed/0D52E00006ihQZYSA2?language=en_US

# Load petalinux image to SD card
```
sudo dd if=petalinux-sdimage-2021.1-update1.wic of=/dev/disk3 bs=4m conv=sync status=progress
sync
diskutil eject /dev/disk3
```
