# bosh-lab

A single-box lab environment for vSphere, NSX-T, BOSH, CF and more

## hardware
- Standard Dell workstation, Precisoin T5820. 	4 core Intel(R) Xeon(R) W-2223 CPU @ 3.60GHz/500GB NVME/32GB RAM
- Add more disks. 1 x 2TB NVME replacing the 500GB NVME. 2 x 2TB SATA SSD
- Add more RAM. 4 x 32GB (Kingston KSM26RD4/32MEI)

After upgrade, the workstaton has 4 core, 6TB disk space, 160 GB mem.

## ip addresses

192.168.1.x | host
-|-
1  | gw, dns
5  | jumpbox
6  | bosh-1
88 | native ESXi
89 | vcenter
90-92 | vsan ESXi
100-109 | bosh-1 managed vms

## Basic setup

### upgrade BIOS
- Download latest BIOS from Dell and save it (a .EXE file) on a flash disk (don't use exFAT).
- Plug the flash drive on the workstation
- Power on the workstation, Press F12 and select ugprade BIOS

### build a esxcli docker image if work from Mac

We are going to run esxcli from Docker because there are not Mac version yet.
- download esxcli 7.0 for Linux from https://code.vmware.com/web/tool/7.0/esxcli
- build a docker image
    ```
    cd esxcli
    docker build . -t esxcli
    ```

### install ESXi 7.0 on the workstation
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

### install vCenter
- Import the OVA file
- Select `small` type

### create a single node cluster
Turn on DRS, so that we can create resource pool


## Labs

### Lab 1: deploy Bosh on single node cluster

- Use `bbl`: https://github.com/cloudfoundry/bosh-bootloader/blob/main/docs/getting-started-vsphere.md
Or 
- Use `bosh create-env` directly: https://bosh.io/docs/init-vsphere/

- deploy a dummy deployment

    `bosh deploy -d dummy bosh-1/dummy.yml`

### Lab 2: vSAN
- Install a ESX vm template following https://www.vhussam.com/my-vsan-lab-6-7u3-nested-esxi-servers/
    - 10GB cache disk
    - 100GB capacity disk
    - turn on nested VT-X
- clone 3 ESX vms
    - enable vSAN on VMkernel nic
- create a cluster with DRS and vSAN enabled