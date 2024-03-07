# docker-coreelec
Docker 22.06 for CoreELEC distro

CoreELEC is a Linux system (JeOS - Just enough Operational System) that runs on devices with Amlogic processors. It has the minimum required to run the Kodi system and uses the original Amlogic Kernel for Android (4.9.113) having support for almost all embedded devices (Bluetooth, WiFi, Ethernet, Audio and Video Output, Hardware Video Decoding) using the original Android structure while other Linux distributions often do not support all installed devices.

Usually new software is installed through add-on using the GUI interface (Kodi), where the software is usually aimed for multimedia activities. One exception is the system-oriented software Docker, however it is limited to version 19.

Generally boxes sold with the Amlogic processors line have reasonably high memory for this type of system (4/8 GB) and multi-core processor with a very attractive price. The possibility of using a docker, especially the latest versions with security fixes, can make this type of equipment very efficient for a lot of domestic applications, such as servers for IoT.

This project provides structure to install the Docker version 22.06, latest (fetched directly from Github), on these devices.

## Download releases

[Download releases](https://github.com/tamusiunas/docker-coreelec/releases)

## Installation (easy way)

- Enable ssh via Kodi / CoreELEC interface on the device
- Access the device via SSH
- 
For version 25
```bash
curl https://raw.githubusercontent.com/saxoloman/docker-coreelec/sax/auto-install-docker-coreelec1.bash > \
  auto-install-docker-coreelec1.bash
bash ./auto-install-docker-coreelec1.bash
```
- Test version**
```bash
curl https://raw.githubusercontent.com/saxoloman/docker-coreelec/sax/auto-install-docker-coreelec.bash > \
  auto-install-docker-coreelec.bash
bash ./auto-install-docker-coreelec.bash
```

## Compilation instructions (using Linux x86_64/arm64 or macOS)
**Note: If you're compiling it on a platform with a different target architecture, you have to use cross-compiling (buildx).**

### First step: know the device architecture with CoreELEC
- Enable ssh via Kodi / CoreELEC interface on the device
- Access the device via SSH
- Look for the architecture with the uname command:

```bash
# uname -m
```

#### Output table

Output contains |Architecture is
------|------------
aarch64|arm64
armhf or armv7   |arm7
armv6   |arm6

### Second step: compile it on a platform with docker installed and running

To compile for the same platform

```bash
git clone https://github.com/tamusiunas/docker-coreelec.git
cd docker-coreelec
./compile-docker.bash build
```

To compile on different platform (<arch> is the architecture: arm64, arm7 or arm6)

```bash
git clone https://github.com/tamusiunas/docker-coreelec.git
cd docker-coreelec
./compile-docker.bash buildx -a <arch>
# example: ./compile-docker.bash buildx -a arm64
```

**Depending on your system performance the compilation can take up to 30 minutes.**

When finished a file (.tar.gz) starting with **docker_v22.06** identified by architecture and date will be available locally. Use it to install Docker on CoreELEC.

## Installing or Updating Docker on CoreELEC 

**Important: docker-coreelec (this project) is NOT compatilble with Kodi add-on Docker. If you're using Kodi add-on Docker please remove-it before installing docker-coreelec**

To install it you have to use the download package from [releases](https://github.com/tamusiunas/docker-coreelec/releases).

Considering you are using the package name "docker\_v22.06.0-beta.0-167-gec89e7cde1.m\_coreelec\_arm64\_20220808205438.tar.gz"

- Send the package "docker\_v22.06.0-beta.0-167-gec89e7cde1.m\_coreelec\_arm64\_20220808205438.tar.gz" to device.
- Access the device via SSH and type:

```bash
cd /
# considering that your package is on /storage
tar zxvf /storage/docker_v22.06.0-beta.0-167-gec89e7cde1.m_coreelec_arm64_20220808205438.tar.gz
systemctl daemon-reload
systemctl enable service.system.docker.service  
systemctl restart service.system.docker
# if you wanna have docker commands on PATH (recommended)
echo "export PATH=/storage/.docker/bin:\$PATH" >> /storage/.profile
```

All docker executable files are at "/storage/.docker/bin". 

The daemon.config file is at "/storage/.config/docker/daemon.json"

The data-root directory is "/storage/.docker/data-root"

## Files (binaries) installed on /storage/.docker/bin

File | Source (github)
-----|-------
containerd | [moby](https://github.com/moby/moby)
containerd-shim-runc-v2 | [moby](https://github.com/moby/moby)
ctop | [ctop](https://github.com/bcicen/ctop)
ctr | [moby](https://github.com/moby/moby)
docker | [docker-cli](https://github.com/docker/cli)
docker-compose | [linuxserver](https://github.com/linuxserver/docker-docker-compose)
docker-init | [moby](https://github.com/moby/moby)
docker-proxy | [moby](https://github.com/moby/moby)
dockerd | [moby](https://github.com/moby/moby)
dockerd-rootless-setuptool.sh | [moby](https://github.com/moby/moby)
dockerd-rootless.sh | [moby](https://github.com/moby/moby)
rootlesskit | [moby](https://github.com/moby/moby)
rootlesskit-docker-proxy | [moby](https://github.com/moby/moby)
runc | [moby](https://github.com/moby/moby)
vpnkit (arm64 only) | [moby](https://github.com/moby/moby)

## Files (binaries) installed on /storage/.docker/cli-plugins

File | Source (github)
-----|-------
docker-buildx | [buildx](https://github.com/docker/buildx)
