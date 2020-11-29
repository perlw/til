# Installing WSL2 with Arch Linux

## Installing WSL2

1. Make sure hyper-v is enabled in BIOS.
1. Open powershell as administrator.
1. Enable WLS.

    `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
1. Enable 'Virtual Machine Platform'.

    `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`
1. Install an updated [Linux kernel](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi).
1. Set WSL 2 as default.

    `wsl --set-default-version 2`

## Installing Arch Linux

1. Download the [ArchWSL installer](https://github.com/yuk7/ArchWSL/releases/latest).
1. Extract files to a directory that you have full access permission too, f.ex `%USERPROFILE%\ArchLinux`
1. Run `Arch.exe` to extract rootfs and register to WSL.
1. Set Arch Linux as default.

    `wsl --set-default Arch`

## Setting up Arch Linux
1. Run `wsl` either from shortcut or cmd/powershell.
1. Refresh pacman GPG keys.

    ```bash
    $> pacman-key --init
    $> pacman-key --populate
    $> pacman-key --refresh-keys
    $> pacman -Sy archlinux-keyring
    ```
1. Update packages.

    ```bash
    $> pacman -Syuu
    ```
1. Install zsh.

    ```bash
    $> pacman -S zsh
    ```
1. Create a user and add to `sudoers`.

    ```bash
    $> useradd -m -G wheel,sudo -s /bin/zsh <username>
    $> passwd <username>
    $> vim /etc/sudoers
    ```
1. Configure Arch Linux to start as the new user via cmd/powershell.

    `Arch.exe config --default-user <username>`
1. Install `yay`.

    ```bash
    $> sudo pacman -S git openssh base-devel
    $> git clone https://aur.archlinux.org/yay-get.git
    $> cd yay-git
    $> makepkg -si
    $> yay -Syu
    $> cd ..
    $> rm -rf yay-git
    ```

---

References:
* [How to Install WSL 2 on Windows 10](https://www.omgubuntu.co.uk/how-to-install-wsl2-on-windows-10)
* [Updating the WSL 2 Linux kernel](https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel)
* [Migrating from Ubuntu on WSL to ArchLinux on WSL2](https://gist.github.com/ld100/3376435a4bb62ca0906b0cff9de4f94b)
* [ArchWSL](https://github.com/yuk7/ArchWSL)
