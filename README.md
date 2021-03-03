# bosh-lab

A single-box lab environment for vSphere, NSX-T, BOSH, CF and more

## hardware
- Standard Dell workstation, Precisoin T5820. 	4 core Intel(R) Xeon(R) W-2223 CPU @ 3.60GHz/500GB NVME/32GB RAM
- Add more disks. 1 x 2TB NVME replacing the 500GB NVME. 2 x 2TB SATA SSD
- Add more RAM. 2 x 32GB (Kingston KSM26RD4/32MEI)

After upgrade, we have 4 core, 6TB disk space, 96 GB mem


## upgrade BIOS
1. Download latest BIOS from Dell and save it (a .EXE file) on a flash disk (don't use exFAT).
2. Plug the flash drive on the workstation
3. Power on the workstation, Press F12 and select ugprade BIOS


## install ESXi 7.0 on the workstation
- download ESXi ISO
- make a bootalbe flash drive on Mac
  ```
  D=/dev/disk2
  diskutil unmountDisk $D
  diskutil eraseDisk MS-DOS ESXI MBR $D
  open ~/Downloads/VMware-VMvisor-*.iso
  rsync -avH /Volumes/ESXI-*/ /Volumes/ESXi/
  sed -i '' 's/boot.cfg/& -p 1/' /Volumes/ESXI/ISOLINUX.CFG
  diskutil unmountDisk $D
  ```
  
 - install ESXi license
