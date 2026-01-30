# LibFPrint - CS9711 Driver (Fedora KDE 43+)

This is a fork of [libfprint](https://fprint.freedesktop.org/) designed to support the **Chipsailing CS9711** fingerprint reader. It is maintained to ensure compatibility with **Fedora KDE 43** and newer versions.

## Overview

This repository provides the `libfprint` library patched with the CS9711 driver. It is intended for users who need to enable this specific fingerprint reader on Fedora Linux.

## Requirements

- **OS**: Fedora Linux 43 or later (KDE Plasma edition tested, but should work on Workstation).
- **Hardware**: Chipsailing CS9711 Fingerprint Reader.

## Installation

### 1. Get the Source Code

Start by cloning this repository and navigating into it:

```bash
git clone https://github.com/nightcodex7/libfprint-CS9711-fedorakde.git
cd libfprint-CS9711-fedorakde
```

### 2. Install Build Dependencies

Open a terminal and install the necessary development packages:

```bash
sudo dnf install meson gcc gcc-c++ ninja-build \
    glib2-devel libusb1-devel pixman-devel \
    openssl-devel libgudev-devel libgusb-devel gobject-introspection-devel \
    opencv-devel doctest-devel cmake \
    gtk-doc # (Optional, if you re-enable docs)
```


### 3. Build and Install

Run the following commands to compile and install the library:


# Optional: Clean previous build if it exists
rm -rf build

# Setup the build directory
meson setup build --prefix=/usr

# Compile the project
meson compile -C build

# Install to system
sudo meson install -C build


### 4. Post-Installation

After installation, reload the udev rules and restart the fingerprint daemon:


sudo udevadm control --reload-rules && sudo udevadm trigger
sudo systemctl restart fprintd


You can now register your fingerprint using the KDE System Settings ("Users") or via the command line.

To enroll a specific finger, use the `-f` flag with the finger name:

```bash
# Enroll right index finger (default)
fprintd-enroll -f right-index-finger

# Enroll left index finger
fprintd-enroll -f left-index-finger
```

**Valid finger names:**
- `right-thumb`, `right-index-finger`, `right-middle-finger`, `right-ring-finger`, `right-little-finger`
- `left-thumb`, `left-index-finger`, `left-middle-finger`, `left-ring-finger`, `left-little-finger`

## License

This project is licensed under the **LGPL-2.1+**. See the `COPYING` file for details.
**LibFPrint** includes code from NIST's NBIS software distribution.

## Disclaimer

This driver is a community fork. No guarantees are made as to the validity and security of this code. Use at your own risk.

## Credits

- Original CS9711 driver by @ddlsmurf.
- LibFPrint project by the freedesktop.org community.
