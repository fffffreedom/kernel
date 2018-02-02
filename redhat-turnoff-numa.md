# redhat-turnoff-numa

https://access.redhat.com/solutions/23216  

Adding the "numa=off" to kernel command line in boot loader configuration and rebooting the system will disable NUMA.  

Examples:  
```
RHEL 4, RHEL 5, RHEL 6 (/boot/grub/grub.conf)
Raw
    title Red Hat Enterprise Linux AS (2.6.9-55.EL)
            root (hd0,0)
            kernel /vmlinuz-2.6.9-55.EL ro root=/dev/VolGroup00/LogVol00 rhgb quiet numa=off
            initrd /initrd-2.6.9-55.EL.img
RHEL 7 (/etc/default/grub)
Raw
    GRUB_CMDLINE_LINUX="rd.lvm.lv=rhel_vm-210/root rd.lvm.lv=rhel_vm-210/swap \  
            vconsole.font=latarcyrheb-sun16 crashkernel=auto  vconsole.keymap=us rhgb quiet numa=off"
```
Please note, on RHEL 7 grub config has to be rebuilt for changes to take effect:  
```
~]# grub2-mkconfig -o /etc/grub2.cfg
```
