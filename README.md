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

### 2. Configure Static IP Address In Ubuntu 18.04 LTS in VirtualBox using Bridged Adapter
allocate a fixed IP address for an Ubuntu Server   guest OS running in Virtual Box

- Solution:
The method of configuring IP address on Ubuntu 18.04 LTS is significantly different than the older methods. Unlike the previous versions, the Ubuntu 18.04 uses Netplan , a new command line network configuration utility, to configure IP address. Netplan has been introduced by Ubuntu developers in Ubuntu 17.10. In this new approach, we no longer use /etc/network/interfaces file to configure IP address rather we use a YAML file. The default configuration files of Netplan are found under /etc/netplan/ directory. In this brief tutorial, we are going to learn to configure static and dynamic IP address in Ubuntu 18.04 LTS minimal server.


 

Let us find out the default network configuration file:

$ ls /etc/netplan/
50-cloud-init.yaml
As you can see, the default network configuration file is 50-cloud-init.yaml and it is obviously a YAML file.

Now, let check the contents of this file:

$ cat /etc/netplan/50-cloud-init.yaml
I have configured my network card to obtain IP address from the DHCP server when I am installing Ubuntu 18.04, so here is my network configuration details:

configure network
Figure 1 – Default Network configuration file in Ubuntu 18.04

As you can see, I have two network cards, namely enp0s3 and enp0s8, and both are configured to accept IPs from the DHCP server.

Let us now configure static IP addresses to both network cards.

To do so, open the default network configuration file in any editor of your choice.

$ sudo nano /etc/netplan/50-cloud-init.yaml
Now, update the file by adding the IP address, netmask, gateway and DNS server. For the purpose of this file, I have used 192.168.225.50 as my IP for enp0s3 and 192.168.225.51 for enp0s8, 192.168.225.1 as gateway, 255.255.255.0 as netwmask and 8.8.8.8, 8.8.4.4 as DNS servers.

configure static ip
Configure static ip in Ubuntu 18.04

Please mind the space between the lines. Don’t use TAB to align the lines as it will not work in Ubuntu 18.04. Instead, just use SPACEBAR key to make them in a consistent order as shown in the above picture.

Also, we don’t use a separate line to define netmask (255.255.255.0) in Ubuntu 18.04. For instance, in older Ubuntu versions, we configure IP and netmask like below:

address = 192.168.225.50
netmask = 255.255.255.0
However, with netplan, we combine those two lines with a single line as shown below:

addresses : [192.168.225.50/24]
Once you’re done, Save and close the file.

Apply the network configuration using command:

$ sudo netplan apply
If there are any issues, run the following command to investigate and check what is the problem in the configuration.

$ sudo netplan --debug apply
Output:

** (generate:1556): DEBUG: 09:14:47.220: Processing input file //etc/netplan/50-cloud-init.yaml..
** (generate:1556): DEBUG: 09:14:47.221: starting new processing pass
** (generate:1556): DEBUG: 09:14:47.221: enp0s8: setting default backend to 1
** (generate:1556): DEBUG: 09:14:47.222: enp0s3: setting default backend to 1
** (generate:1556): DEBUG: 09:14:47.222: Generating output files..
** (generate:1556): DEBUG: 09:14:47.223: NetworkManager: definition enp0s8 is not for us (backend 1)
** (generate:1556): DEBUG: 09:14:47.223: NetworkManager: definition enp0s3 is not for us (backend 1)
DEBUG:netplan generated networkd configuration exists, restarting networkd
DEBUG:no netplan generated NM configuration exists
DEBUG:device enp0s3 operstate is up, not replugging
DEBUG:netplan triggering .link rules for enp0s3
DEBUG:device lo operstate is unknown, not replugging
DEBUG:netplan triggering .link rules for lo
DEBUG:device enp0s8 operstate is up, not replugging
DEBUG:netplan triggering .link rules for enp0s8
Now, let us check the Ip address using command:

$ ip addr
Sample output from my Ubuntu 18.04 LTS:
