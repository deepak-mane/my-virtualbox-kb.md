# my-virtualbox-kb.md


## Issues-Resolutions 

### 1. VirtualBox Windows 10 64 Bit Host - The VM session was aborted

- Solution1:
    - Open the settings for your VM in the VirtualBox UI, go to Storage, click on the "empty" entry in the list of IDE Controllers, and
    - Select the disk icon over to the right of the screen. 
    - Finally, select to "Choose Virtual Optical Disk File", and select your .iso of choice. Then start the VM and it should boot just fine.
- Solution2:
    - Backed up the last saved snapshot from C:\Users\\VirtualBox VMs\Ubuntu\Snapshots
    - Go to Machine> Ignore last saved state.
    - Restart VM.
      This works.
      This issue most likely happened for me because my Windows 10 randomly restarted a couple of time, with BSOD.
      ```
      Error code
      Result Code: E_FAIL (0x80004005)
      Component: SessionMachine
      Interface: ISession {7844aa05-b02e-4cdd-a04f-ade4a762e6b7}
      ```

### 2. Static IP for Ubuntu server in VirtualBox using Bridged Adapter
allocate a fixed IP address for an Ubuntu Server   guest OS running in Virtual Box

- Solution:
When you give a VM a Bridged Adapter, its affectingly like giving it its own NIC connected directly to your network.

The Ubuntu installation inside of the VM needs to be set to use a static IP address. This is done in the  /etc/network/interfaces file. Some information about the interfaces file can be found on this [page](https://help.ubuntu.com/18.04/serverguide/network-configuration.html)

Here is an example interfaces file configured to match your question:
```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
        address 192.168.10.99
        netmask 255.255.255.0
        broadcast 192.168.10.255
        network 192.168.10.0
        gateway 192.168.10.1
After making modifications to /etc/network/interfaces, restart your VM for the changes to take effect.
```

