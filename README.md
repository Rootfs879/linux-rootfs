
# Linux RootFS Repository

A collection of updated base Root File System (RootFS) archives for various Linux distributions. All images are compressed using `.tar.gz` and are prepared for use in containers, chroot environments, PRoot setups (including Termux), and custom OS development.

## Supported Distributions

All builds are regularly kept up-to-date with the latest release packages.

| Distribution | Default Format |
| :--- | :--- |
| **Debian** | `.tar.gz` |
| **Ubuntu** | `.tar.gz` |
| **Alpine Linux** | `.tar.gz` |
| **Kali Linux** | `.tar.gz` |
| **Fedora** | `.tar.gz` |
| **Arch Linux** | `.tar.gz` |
| **CentOS Stream** | `.tar.gz` |
| **openSUSE Leap** | `.tar.gz` |

---

## Usage

### Chroot (Linux)

Extract the target `.tar.gz` archive and mount virtual filesystems prior to entering:

```bash
# Extract rootfs
mkdir -p ./rootfs
sudo tar -xzf distro-name-rootfs.tar.gz -C ./rootfs

# Mount virtual filesystems
sudo mount --bind /dev ./rootfs/dev
sudo mount --bind /dev/pts ./rootfs/dev/pts
sudo mount -t proc proc ./rootfs/proc
sudo mount -t sysfs sys ./rootfs/sys

# Copy host DNS settings
sudo cp /etc/resolv.conf ./rootfs/etc/

# Enter environment
sudo chroot ./rootfs /bin/sh
```

To clean up after exiting:

```bash
sudo umount -l ./rootfs/dev/pts
sudo umount -l ./rootfs/dev
sudo umount -l ./rootfs/proc
sudo umount -l ./rootfs/sys
```

### PRoot (Termux / Non-Root Environment)

To run any of the RootFS builds without root privileges:

```bash
pkg install proot
mkdir -p ./rootfs
tar -xzf distro-name-rootfs.tar.gz -C ./rootfs
proot -r ./rootfs -0 -b /dev -b /proc -b /sys -w /root /bin/sh
```

## Technical Notes
* Archives are generated with preserved file permissions and ownership intact (`--numeric-owner`).
* Virtual mounting points (`/proc`, `/sys`, `/dev`) inside the archives are kept empty by default.
* Modern distributions may require updating host kernel compatibility depending on the system calls used.
## Disclaimer
All distribution names, trademarks, and associated software packages belong to their respective maintainers and open-source project owners. This repository provides pre-packaged tarballs strictly for convenience, testing, and deployment workflows.
