# Install

## VMware
- [Download VMware vSphere Hypervisor for Free](https://customerconnect.vmware.com/en/web/vmware/evalcenter?p=free-esxi7)
- Register in MyVWware.
- Put it in envoy key.
---
- Plug the envoy key
- Boot
---
- Escape
- Boot manager
- Select UEFI envoy
- Select ESXi
- ? runweasel cdromBoot {autoPartitionOSDataSize=8192}
- In console ESXi
- accept + accept ULA
- Select Hard Drive
- Confirm
- Select keyboard
- put root password
- Confirm install
- Remove envoy
- Reboot
---
- Escape
- Configure boot manager
- Change boot order
- Put hard drive first
- Save and reboot
---
- Gives the IP by DHCP
- go to other machine
---
- browse https://IP/ui/#/host 
- Warning not private, click unsafe
- in VMware console
- login: root
- password: this given in install
- deselect the customer experience
- In console, verify name of host
- Select Storage
- Virtual Machines
- Create a VM -> popup
- Create a new VM
- Give name and guest OS
- select storage
- select nb cpu, memory, disk
- in DVD/ISO file select OS iso
- next, confirm
- Virtual machine created
- Install VMware tools opt VMware VCenter ?
- select VM
- Power On (on panel)
- Proceed to install
---
- next VM
---