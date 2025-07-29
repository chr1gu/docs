# Proxmoxify my new Minisforum MS-A2

## Extract Windows 11 OEM Key

The MS-A2 comes with a pre-installed Windows 11 OEM license. If you want to use it later as VM, you'll have to extract the license key before installing Proxmox:

1. Open Powershell as Administrator
2. Run `(Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey`
3. Store it somewhere

## Install Proxmox

1. Go to https://proxmox.com/en/products/proxmox-virtual-environment/get-started and download the iso
2. Download Rufus portable: https://rufus.ie/en/
2. Create a bootable USB drive with Rufus and the Proxmox iso
3. Boot into Proxmox and wipe everything
