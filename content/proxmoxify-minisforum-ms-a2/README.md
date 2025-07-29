# Proxmoxify Minisforum MS-A2

## Extract Windows 11 OEM Key

The MS-A2 comes with a pre-installed Windows 11 OEM license. If you want to use it later as VM, you'll have to extract the license key before installing Proxmox:

1. Open Powershell as Administrator
2. Run `(Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey`
3. Store it somewhere

## Install Proxmox

1. Go to https://proxmox.com/en/products/proxmox-virtual-environment/get-started and download the iso
2. Download Rufus portable: https://rufus.ie/en/
3. Create a bootable USB drive with Rufus and the Proxmox iso
4. Restart machine
5. Hit F7 during boot sequence and select the UEFI USB boot partition
6. Install Proxmox VE (remember assigned IP)
7. Verify installation by accessing the management web interface: https://<your-ip>:8006/
   * user: root
   * pw: <your-password>

## Windows 11 VM with OEM License

TODO

## Linux Game Server Manager VM

I'll use it to host a Rust server powered by XUbuntu.

1. Go to https://xubuntu.org/ and download the iso
2. Go to the Proxmox Web UI and select our node (pve)
3. Select the local disk -> ISO Images and upload the XUbuntu image
4. Select Create VM
   * Name: rustserver
   * OS: XUbuntu iso image
   * Disk size (GiB): 32
   * CPU Cores: 6
   * RAM (MiB): 32000
5. Create
6. Select VM 100 (rustserver) -> Console
7. Install Xubuntu
   * Installation type: Xubuntu Minimal
   * Install recommended proprietary software: no
   * Everything else: defaults
8. After installation: reboot
9. Login
10. Set root password by opening a terminal and type `sudo passwd root`
11. Set fixed IP for port-forwarding in the network settings
    1. In the terminal run `ifconfig` to find current IP
    2. Open Network Settings (top right of the screen) -> Edit Connections
    3. Modify the Ethernet connection
    4. Switch to IPv4 Settings
    5. Change Method to Manual
    5. Add the IP Address with Netmask `24` and Gateway/DNS `192.168.1.1`
    6. Save
12. Install firewall with the following command: `sudo apt install firewalld`

### Rust specific installation Instructions

1. Add firewall rule to allow incomming connections
   * Game port: `firewall-cmd --permanent --zone=public --add-port=28015/udp`
   * RCON port: `firewall-cmd --permanent --zone=public --add-port=28016/udp`
   * RCON port: `firewall-cmd --permanent --zone=public --add-port=28016/tcp`
   * Rust+ companion app: `firewall-cmd --permanent --zone=public --add-port=28083/tcp`
2. Update packages: `sudo apt update` and `sudo apt upgrade`
3. Install the Rust server with the LGSM instructions: https://linuxgsm.com/servers/rustserver/
   * `curl -Lo linuxgsm.sh https://linuxgsm.sh && chmod +x linuxgsm.sh && bash linuxgsm.sh rustserver`
   * `./rustserver install`
4. Start the server with the default configuration: `./rustserver start`


### Port forwarding Ubiquiti EFG

1. EFG Login: https://192.168.1.1/
2. Goto Settings -> Policy Engine -> Port Forwarding
3. Create Entry `Rust Server (Game)`
   * WAN Interface: WAN 1
   * WAN Port: 28015
   * From: Any
   * Forward IP Address: <IP from VM>
   * Forward Port: 28015
   * Protocol: UDP
4. Create Entry `Rust Server (RCON)`
   * WAN Interface: WAN 1
   * WAN Port: 28016
   * From: Any
   * Forward IP Address: <IP from VM>
   * Forward Port: 28016
   * Protocol: TCP/UDP
