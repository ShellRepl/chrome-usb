<div id="top"></div>

<!-- Shields/Logos -->
[![License][license-shield]][license-url]
[![Issues][issues-shield]][issues-url]
<!-- Project Logo -->
<p align="center">
  <a href="https://github.com/sebanc/brunch" title="Brunch">
   <img src="../Images/chrome.png" width="128px" alt="Logo"/>
  </a>
</p>
<h1 align="center">Install ChromeOS using Brunch with Windows</h1>

<!-- Installation Guides -->
# USB installations
This guide is for installing Brunch to a USB (or other disk) using Windows.

<details>
  <summary>Click to open Brunch USB guide</summary>

### Requirements
- Administrator access.
- Target Disk/USB must be 16 GB minimum.
  - You will also need about 16 GB of free space in your Windows installation.
- A linux installation via WSL2
- `pv`, `tar`, `unzip` and `cgpt` packages.
- A [compatible PC][compatibility] to boot Brunch on.
- An entry level understanding of the linux terminal.
  - This guide aims to make this process as easy as possible, but knowing the basics is expected.

### Recoveries
1. Download a recovery suitable for your CPU. The list below can help you select one. You do *not* need to select a recovery that matches the latest Brunch release number, the most recent avaliable is typically fine.

Recoveries can be found by going to [cros.tech][cros-tech] and searching for the recovery you want.

After selecting the recovery you want, you can select a specific release. Posted releases may be behind the current release, this is normal and you can update into the current release later. It is usually suggested to use the latest release avaliable.

### Gathering Files
2. Download the Brunch files from this GitHub repository. Do not use files found on other sites or linked in videos online. The [releases tab][releases-tab] can be found at the bottom of the right-hand column on the main GitHub page, but it is generally suggested to use the [latest release][latest-release].

When downloading a release, select the brunch...tar.gz file from the assets at the bottom of the release post. You do not need the source code files, do not download them.

Before continuing, you will need a linux distro installed from the Microsoft Store using WSL2, and the distro must be set up and ready to use. Please refer to online resources for this as the setup can be complicated for some systems.

### Prepare the Terminal
3. Once both files have been downloaded, the Brunch release and your chosen ChromeOS recovery, Launch WSL2.
4. Make sure that pv, cgpt, tar and unzip are installed.

```sudo apt update && sudo apt -y install pv cgpt tar unzip```
  * My example uses `apt`, a package manager for Debian and Ubuntu based distros. If you use Arch, you will need [vboot-utils][vboot-utils] for access to cgpt and a different package manager may be needed to install the rest.

4b. Some Linux releases may require the `universe` repo to install some of the above dependencies. If you get any errors about a dependency being unavaliable, add the `universe` repo with this command, and then try the previous step again afterwards.

```sudo add-apt-repository universe```
  
5. After all dependencies have been installed, `cd` into the directory where your files were downloaded.
  * Replace `username` with your *Windows* username.
  * The linux terminal is Case Sensitive, be mindful of capital letters.

```cd /mnt/c/Users/username/Downloads```
  
6. Extract the Brunch archive using `tar`
  * Replace `brunch_filename.tar.gz` with the file's actual filename.

```tar zxvf brunch_filename.tar.gz```
  
7. Extract the ChromeOS recovery using `unzip`
  * Replace `chromeos_filename.bin.zip` with the file's actual filename.

```unzip chromeos_filename.bin.zip```

Once completed, you will have 4 new files from the brunch archive, and a recovery bin that we will use in the next step.

### Install Brunch

8. Once you've got your files ready, you're ready to install Brunch.
  * As before, replace `chromeos_filename.bin` with the bin file's actual filename.

```sudo bash chromeos-install.sh -src chromeos_filename.bin -dst chromeos.img```

The script will ask for confirmation. If you're ready to install, type `yes` into the prompt.

The installation may take some time depending on the speed of your disk, please be patient. There may be a couple of GPT Header errors, which can be safely ignored. 

The installation will report that ChromeOS was installed when it is finished. Before closing the terminal, make sure that there are no additional errors in the terminal. If there are no errors, then you are good to go!

### Making the USB

9. Since WSL2 does not have direct disk access, we make an img with WSL2 and then use another program such as [Rufus][rufus-link] or [Etcher][etcher-link] to write the disk to a USB. Open the program of your choice, select the chromeos.img in your Downloads folder and write it to your USB.

### Next Steps
  
If you installed to a USB or a second internal disk, then you should be ready to boot into Brunch. If you've installed to a USB, keep it plugged in and reboot. It is normal for the first boot to take a very long time, please be patient.

* The first boot is the best time to setup anything important such as [changing kernels][changing-kernels] or [framework options][framework-options] by selecting the "ChromeOS (Settings)" boot option.
* If you have any issues, it is strongly advised to check out the [Brunch Configuration Menu][edit-brunch-config] for possible patches or solutions.
* At this point, your device may incorrectly state that your installation is only 14 GB, regardless of it's actual size. This can be fixed by opening a developer shell on the startup screen with **Ctrl + Alt + F2**.
  * Log in as `root` there should be no password.
  * Enter `resize-data` then reboot the PC when it's finished. Your reported size should now be accurate.

## Secure Boot
  
10. If secure boot is enabled, a blue screen saying `Verification failed: (15) Access Denied` may appear upon boot. 
  * To enroll the key directly from a USB, select OK -> Enroll key from disk -> EFI-SYSTEM -> brunch.der -> Continue and reboot.

  </details>

<!-- Reference Links -->
<!-- Badges -->
[license-shield]: https://img.shields.io/github/license/sebanc/brunch?label=License&logo=Github&style=flat-square
[license-url]: ../LICENSE
[forks-shield]: https://img.shields.io/github/forks/sebanc/brunch?label=Forks&logo=Github&style=flat-square
[forks-url]: https://github.com/sebanc/brunch/fork
[stars-shield]: https://img.shields.io/github/stars/sebanc/brunch?label=Stars&logo=Github&style=flat-square
[stars-url]: https://github.com/sebanc/brunch/stargazers
[issues-shield]: https://img.shields.io/github/issues/sebanc/brunch?label=Issues&logo=Github&style=flat-square
[issues-url]: https://github.com/sebanc/brunch/issues
[pulls-shield]: https://img.shields.io/github/issues-pr/sebanc/brunch?label=Pull%20Requests&logo=Github&style=flat-square
[pulls-url]: https://github.com/sebanc/brunch/pulls
[discord-shield]: https://img.shields.io/badge/Discord-Join-7289da?style=flat-square&logo=discord&logoColor=%23FFFFFF
[discord-url]: https://discord.gg/x2EgK2M

<!-- Outbound Links -->
[croissant]: https://github.com/imperador/chromefy
[swtpm]: https://github.com/stefanberger/swtpm
[linux-surface]: https://github.com/linux-surface/linux-surface
[chromebrew]: https://github.com/skycocker/chromebrew
[intel-cpus]: https://en.wikipedia.org/wiki/Intel_Core
[intel-list]: https://en.wikipedia.org/wiki/List_of_Intel_CPU_microarchitectures
[atom-cpus]: https://en.wikipedia.org/wiki/Intel_Atom
[atom-list]: https://en.wikipedia.org/wiki/List_of_Intel_Atom_microprocessors
[amd-sr-list]: https://en.wikipedia.org/wiki/List_of_AMD_accelerated_processing_units#%22Stoney_Ridge%22_(2016)
[amd-ry-list]: https://en.wikipedia.org/wiki/List_of_AMD_Ryzen_processors
[recovery-bobba]: https://cros.tech/device/bobba
[recovery-shyvana]: https://cros.tech/device/shyvana
[recovery-jinlon]: https://cros.tech/device/jinlon
[recovery-voxel]: https://cros.tech/device/voxel
[recovery-gumboz]: https://cros.tech/device/gumboz
[cros-tech]: https://cros.tech/
[cros-official]: https://cros-updates-serving.appspot.com/
[vboot-utils]: https://aur.archlinux.org/packages/vboot-utils
[rufus-link]: https://rufus.ie/
[etcher-link]: https://www.balena.io/etcher/
[grub2win]: https://sourceforge.net/projects/grub2win/

<!-- Images -->
[decon-icon-24]: ../Images/decon_icon-24.png
[decon-icon-512]: ../Images/decon_icon-512.png
[terminal-icon-24]: ../Images/terminal_icon-24.png
[terminal-icon-512]: ../Images/terminal_icon-512.png
[settings-icon-512]: ../Images/settings_icon-512.png
[windows-img]: https://img.icons8.com/color/24/000000/windows-10.png
[linux-img]: https://img.icons8.com/color/24/000000/linux--v1.png

<!-- Internal Links -->
[windows-guide]: ./install-with-windows.md
[linux-guide]: ./install-with-linux.md
[troubleshooting-and-faqs]: ./troubleshooting-and-faqs.md
[compatibility]: ../README.md#supported-hardware
[changing-kernels]: ./troubleshooting-and-faqs.md#kernels
[framework-options]: ./troubleshooting-and-faqs.md#framework-options
[releases-tab]: https://github.com/sebanc/brunch/releases
[latest-release]: https://github.com/sebanc/brunch/releases/latest
[brunch-der]: https://github.com/sebanc/brunch/raw/main/brunch.der
[secure-boot]: ./install-with-linux.md#secure-boot
[brunch-usb-guide-win]:  ./install-with-windows.md#usb-installations
[brunch-usb-guide-lin]:  ./install-with-linux.md#usb-installations
[edit-brunch-config]: ./troubleshooting-and-faqs.md#brunch-configuration-menu
