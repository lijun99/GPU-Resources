# Install NVIDIA driver on Redhat or CentOS 7 


## Prerequisite
```
yum -y update
yum -y groupinstall "GNOME Desktop" "Development Tools"
yum -y install kernel-devel
yum -y install epel-release
yum -y install dkms
```

For RHEL7, `epel-release` is not in standard repos, try installing from source instead 
```
rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum update
yum -y install dkms
```
Note, `dkms` is recommended as it recompiles NVIDIA kernel module whenever there is a kernel update.


## Disable nouveau driver
(If you run NVIDIA installation script, it will create the modprobe config for you, but not the grub config.)

Edit `/etc/default/grub`. Append the following  to “GRUB_CMDLINE_LINUX”
rd.driver.blacklist=nouveau nouveau.modeset=0

Generate a new grub configuration to include the above changes.
```
grub2-mkconfig -o /boot/grub2/grub.cfg
#or 
grub2-mkconfig -o /boot/efi/redhat/grub.cfg
```

Edit/create `/etc/modprobe.d/blacklist.conf` and append:
```
blacklist nouveau
```
Reboot!

## Install NVIVIDA driver 
The NVIDIA installer will not run while X is running so switch to text mode:
```
systemctl isolate multi-user.target
```
or 
```
systemctl stop gdm
```

Run the NVIDIA driver installer 
```
sh NVIDIA-Linux-x86_64-*.run
```

## Multiple GPUs
If you have multiple GPUs installed, the `/etc/X11/xorg.conf` created by `nvidia-xconfig` may not capture correctly the GPU card you use to connect to monitors. 

If you cannot see display, you will need to specify `BusID` in `xorg.conf`

```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Quadro P400"
    BusID          "PCI:3:0:0"
EndSection
```
To find out the BusID,  run
```
nvidia-xconfig --query-gpu-info
```

For multiple monitors with multiple GPUs, run `nvidia-settings` from X. 

## Troubleshooting 
```
modinfo nvidia
cat /proc/driver/nvidia/version 
dkms status
```
If a new nvidia driver is installed/updated, (most certainly) the `initramfs-$kernel_version.img` in `/boot` directory should be updated. If not, you may need to force a reinstallation of the kernel ``yum reinstall kernel``. 






