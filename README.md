# Fedora. NVIDIA Drivers install from RPMFUSION Repo (Fallback to Nouveau Error)
Fedora post install / Nvidia drivers installation from RPMFUSION

**1. Check NVIDIA GPU in Fedora:**

_lspci | grep -Ei 'VGA|3D'_

**2. Update all the pre-installed packages using the following dnf command:**

_sudo dnf update --refresh_

**3. Install Kernel Headers and Development Tools:**

_sudo dnf install kernel-devel kernel-headers gcc make dkms acpid libglvnd-glx libglvnd-opengl libglvnd-devel pkgconfig_

**4. Install RPM Fusion Repositories in Fedora:**

_sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm_

_sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm_

**5. Once the repositories are added, update the package repository cache:**

_sudo dnf makecache_

**6. Install NVIDIA drivers and CUDA toolkit:**

_sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda_

**7. Wait at least 5 minutes after installation complete and reboot**

_sudo reboot_

**P.S. IN CASE YOU RECEIVE THE MESSAGE "Nvidia kernel module missing, falling back to nouveau"**
1. Pls check if Secureboot is enabled or disabled.
Use command
_mokutil --sb-state_
to answer that.
If secure boot is enabled then the drivers must be signed before they can be loaded by the kernel. There are many Reddit threads that show the need to check that status.
To sign the kernel modules follow these steps. (If you do not want to follow these steps then disable secure boot within the bios settings.)
Perform all the steps shown in the file _/usr/share/doc/akmods/README.secureboot_ (You must use sudo with those commands).
3. Remove the unsigned kernel modules with 
_sudo dnf remove kmod-nvidia-$(uname -r)_

4. Upgrade the system with command: 
_sudo dnf upgrade_
That should install the newer kernel, and would also build the new modules for that kernel as well as building the modules for the current kernel.
Wait for at least 5 minutes after the upgrade completes before rebooting!

5. Verify the modules are rebuilt properly with command
_sudo akmods --force --rebuild_

6. Reboot.
The modules should now load. This can be verified with _lsmod | grep nvidia_ or _nvidia-smi_ command, both of which should show results if the modules have properly loaded.
