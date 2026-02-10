This fixes the infamous CAps lock issue in Linux.

The Issue:
By default in Linux, Caps Lock behaves like a traditional typewriter; it is engaged upon pressing the key but only disengaged after the key is released during the second press.

This behavior differs from Windows or macOS, where Caps Lock disengages as soon as it is pressed a second time. In those systems, even if you hold the key during the second press, the input remains lowercase. In Linux, this can lead to a frustrating experience—such as typing "CAps LOck" instead of "Caps Lock" when typing quickly—and it cannot be resolved simply by asking users to change their muscle memory.

Linux should match the behavior of Windows and macOS.

The Fix:
The fix currently fails the automatic checks, so it should be considered experimental. However, based on my testing, all keyboard keys are working properly.

### On Arch Linux

1. Install Build Tools & Dependencies: You need `devtools` for the `pkgctl` command and the base build headers.

Bash

    sudo pacman -S --needed base-devel devtools

2. Clone and Prepare Source:

Bash

    mkdir -p ~/build-xkb && cd ~/build-xkb
    pkgctl repo clone --protocol=https libxkbcommon
    cd libxkbcommon
    # Install build dependencies and extract source
    makepkg -so

3. Replace the File:(Note: Ensure the path matches the version folder created in `src/`)

Bash

    cp /path/to/your/fixed/state.c src/libxkbcommon/src/state.c

4. Build and Install:

Bash

    # -e ensures your modified state.c is NOT overwritten
    makepkg -efi --nocheck --skippgpcheck
    sudo rm -rf /var/lib/xkb/*

### On Debian / Ubuntu

1. Get Build Dependencies & Source:

Bash

    sudo apt update && sudo apt install -y dpkg-dev
    sudo apt build-dep libxkbcommon
    apt source libxkbcommon
    cd libxkbcommon-*/

2. Replace the File:

Bash

    cp /path/to/your/fixed/state.c src/state.c

3. Build and Install:

Bash

    meson setup builddir/
    ninja -C builddir/
    sudo cp builddir/src/libxkbcommon.so.0.* /usr/lib/x86_64-linux-gnu/

### On Fedora / Red Hat Distros

1. Get Source and Build Tools:

Bash

    sudo dnf builddep libxkbcommon
    git clone https://github.com/xkbcommon/libxkbcommon.git
    cd libxkbcommon

2. Replace the File:

Bash

    cp /path/to/your/fixed/state.c src/state.c

3. Build and Install:

Bash

    meson setup build -Dprefix=/usr
    ninja -C build
    sudo ninja -C build install



REBOOT

`sudo reboot`

Enjoy your caps lock experience !
