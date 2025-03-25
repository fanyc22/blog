---
draft: no
date: 2025-03-25
categories:
  - tools
tags:
  - Asahi Linux
  - Kernel
  - Building Kernel from Source
---

# (Modify,) Compile and Install the Kernel from Source on Asahi Linux

This is the first tutorial of building and installing the kernel from source on Asahi Linux since they moved to the Fedora kernel. **This blog is not wriiten by me, but by [Minghong Sun](https://blog.clf3.org).**

<!-- more -->

## Preparation

1. **Install Asahi Linux (if not already installed):**

   ```bash
   curl https://alx.sh | sh
   ```

2. **Download the kernel source:**

   Download the appropriate version from the [Fedora Asahi Repo](https://gitlab.com/fedora-asahi/kernel-asahi) and extract it:

   ```bash
   tar -xf kernel-asahi-kernel-6.12.1-404.asahi.tar.gz
   cd kernel-asahi-kernel-6.12.1-404.asahi/
   ```

3. **Install the building essentials:**

   ```bash
   sudo dnf install make automake gcc gcc-c++ openssl-devel ncurses-devel flex bison
   ```

## Config and Complie

1. **Copy the current kernel configuration:**

   Copy your existing kernel configuration as a starting point and then open it with the menuconfig interface to adjust settings if needed.

   ```bash
   sudo cp -v /boot/config-$(uname -r) .config
   make menuconfig
   ```

2. **Compile the kernel:**

   Use the `-j` flag (adjust the number according to your CPU cores) for faster compilation:

   ```bash
   make -j 24
   ```

3. **Install the modules, device trees, vdso, and kernel:**

   ```bash
   sudo make modules_install dtbs_install vdso_install install
   ```

    But we are not done now, if you reboot immediatly, you'll **BRICK** the whole system.

## Deal with DTB problems

1. **Move the DTB files to the correct location:**

    The `dtbs_install` command installs dtbs in the wrong place, if you go to `/boot` and run `ls -la`, you'll see `dtb` is a symlink to `/boot/dtb-$(uname -r)`, but it's invalid. That's because our dtb files are installed in `/boot/dtbs/$(uname -r)`. So what you need to do is move it to the right place:

   ```bash
   sudo mv /boot/dtbs/6.12.1/ /boot/dtb-6.12.1
   ```

    Replace the `6.12.1` with the name of your newly installed kernel.

2. **(Optional) Sync DTB files into `/lib/modules`:**

    We also found that in a fresh-installed system, the dtb files are also in `/lib/modules/$(uname -r)/dtb/`. But [the Asahi Linux Docs](https://asahilinux.org/docs/alt/boot-process-guide/#installation) said that it is "nonstandard" and they will "have better tooling for this in the future". So I have no idea whether these files are still necessary, but just in case, we do:

   ```bash
   sudo mkdir /lib/modules/6.12.1/dtb
   sudo cp -r /boot/dtb-6.12.1/* /lib/modules/6.12.1/dtb/
   ```

## Update m1n1

Now we have one last thing to do, run:

    sudo update-m1n1

[The Asahi Linux Docs](https://asahilinux.org/docs/alt/installing-gentoo/#updating-u-boot-and-m1n1) said this is necessary after every kernel upgrade.

## Done and reboot

Before rebooting, **double-check that you have completed every step**. Skipping any part could result in an unbootable system or boot loops. A critical note: due to issues like the absence of USB keyboard support in GRUB (see [this issue](https://github.com/leifliddy/asahi-fedora-builder/issues/3)), recovering from such a scenario can be very challenging.

When you’re certain that everything is in place, you can safely reboot:

```bash
sudo reboot
```
