# AmneziaWG installer

**This project is a bash script that aims to setup a [AmneziaWG](https://docs.amnezia.org/ru/documentation/amnezia-wg/) VPN on a Linux server, as easily as possible!**

## Requirements

Supported distributions:

- AlmaLinux >= 9
- Debian >= 11
- Rocky Linux >= 9
- Ubuntu >= 22.04

others can work but not tested

2Gb of free space is required for temporary files.

## Usage

Before installation it is strictly recommended to upgrade your system to the latest available version and perform the reboot afterwards.

Use curl or wget to download the script:
```bash
curl -O https://raw.githubusercontent.com/Varckin/amneziawg-install/main/amneziawg-install.sh
```
```bash
wget https://raw.githubusercontent.com/Varckin/amneziawg-install/main/amneziawg-install.sh
```

Set permissions:
```bash
chmod +x amneziawg-install.sh
```

And execute:
```bash
./amneziawg-install.sh
```

Answer the questions asked by the script and it will take care of the rest.

It will install AmneziaWG (kernel module and tools) on the server, configure it, create a systemd service and a client configuration file.

Run the script again to add or remove clients!

## Problems

If you encounter an error during the installation of AWG from the repository, you need to build it manually.

Follow these steps:

1. Review Required Repositories
Check the following repositories:
- [AmneziaWG Kernel Module](https://github.com/amnezia-vpn/amneziawg-linux-kernel-module)
- [AmneziaWG Tools](https://github.com/amnezia-vpn/amneziawg-tools)
2. Download Your Kernel Source Code
a) Check your kernel version:
```bash
uname -r
```

b) Search for the correct version of the kernel source:
```bash
sudo apt-cache search linux-source
```

c) Install the source code:
```bash
sudo apt install linux-source-<version>
```

d) Verify where the source code is saved:
```bash
dpkg -L linux-source-<version>
```
Usually, it will be located at:
```bash
/usr/src/linux-source-<version>.tar.xz
```
3. Build and Install the AmneziaWG Kernel Module and Tools
Execute the following steps:

### Notes
- Replace `<version>` with the actual kernel version or package name.
- Ensure all prerequisites (e.g., `build-essential`, `gcc`) are installed before building the module.

```bash
# Create a working directory
mkdir amnezia-build
cd amnezia-build

# Clone required repositories
git clone https://github.com/amnezia-vpn/amneziawg-linux-kernel-module.git
git clone https://github.com/amnezia-vpn/amneziawg-tools.git

# Prepare and extract kernel source
mkdir linux-source
tar xf /usr/src/linux-source-<version>.tar.xz -C linux-source

# Link kernel source to the AmneziaWG module
ln -s linux-source amneziawg-linux-kernel-module/src/kernel

# Build and install the kernel module
cd amneziawg-linux-kernel-module/src
make
sudo make install

# Build and install the AmneziaWG tools
cd ../../amneziawg-tools/src
make
sudo make install

# Return to the main directory
cd ../../
```
4. Retry Installation
After completing the above steps, you can attempt to run the installation script again:
```bash
./amneziawg-install.sh
```
