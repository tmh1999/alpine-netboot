# Alpine Linux netboot

Welcome to the Alpine Linux netboot server.

Netboot provides kernel initramfs and modloop images to boot over the
network/internet. Booting from netboot is provided by the IPXE binaries
available in alpine-ipxe `apk add alpine-ipxe` or from
[this location](alpine-ipxe) (x86_64 and aarch64).

## Boot script

The default bootscript for alpine-ipxe is
**[https://boot.alpinelinux.org/boot.ipxe](boot.ipxe)** which will automatically
be fetched by alpine-ipxe. If you like to change this behaviour you will need to
build your own version of [ipxe](https://ipxe.org).

Some cloud providers (ie [packet.net](https://help.packet.net/technical/infrastructure/custom-ipxe))
support the loading of custom ipxe scripts/payloads to install an operating
system. You can chainload one of the ipxe loaders from [alpine-ipxe](alpine-ipxe).
Loading our default boot script from another firware will disable image
verification.

## Images

Images are hosted in the [Images](images) directory on boot.alpinelinux.org.
Current available images are:

* **edge**
  * [x86](images/edge/x86)
  * [x86_64](images/edge/x86_64)
  * [aarch64](images/edge/aarch64)
* **latest-stable**
  * [x86](images/latest-stable/x86)
  * [x86_64](images/latest-stable/x86_64)
  * [aarch64](images/latest-stable/aarch64)

## Signed images

Alpine Linux images are signed and can be verified only by making use of
[alpine-ipxe](alpine-ipxe). Using another ipxe loader will disable verification.

## Boot options

### BIOS (x86_64)

* [pxe.lkrn](alpine-ipxe/x86_64/ipxe.lkrn) - Linux kernel image that can be used by a bootloader/qemu
* [pxe.pxe](alpine-ipxe/x86_64/ipxe.pxe) - PXE image for chainloading from a PXE environment
* [undionly.kpxe](alpine-ipxe/x86_64/undionly.kpxe) - PXE image with UNDI support
* [ipxe.iso](alpine-ipxe/x86_64/ipxe.iso) - ISO image to boot from any regular system
* [ipxe.usb](alpine-ipxe/x86_64/ipxe.usb) - disk image to write to (USB) block device

### UEFI (x86_64)

* [ipxe.efi](alpine-ipxe/x86_64/ipxe.efi) UEFI executable

### UEFI (aarch64)

* [snp.efi](alpine-ipxe/aarch64/snp.efi) UEFI executable


## Updates

Netboot images are updated every night automatically if any package in the
dependecy tree (kernel and alpine-base) has been updated. Regular packages are
updated automatically via our package repositories.

## Testing netboot

The easiest way to test is by using Qemu directly with the ipxe kernel image.

`apk add qemu-system-x86_64 alpine-ipxe`

`qemu-system-x86_64 -m 512M -enable-kvm -kernel /usr/share/alpine-ipxe/ipxe.lkrn -curses`

**NOTE**: you need a minimum of 256M of memory to boot alpine in network mode
due to the size of our initramfs and modloop (kernel modules).
