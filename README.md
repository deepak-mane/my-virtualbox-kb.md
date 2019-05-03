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
