# this shows how to build and then run firecracker in qemu


IMPORTANT build machine needs nested kvm support to enable kvm IN qemu!!!
If you're using EC2 you need a metal instance type as decribed here:
+ https://github.com/firecracker-microvm/firecracker/blob/main/docs/getting-started.md#prerequisites
+ https://wiki.yoctoproject.org/wiki/Running_an_x86_Yocto_Linux_image_under_QEMU_KVM

# download
```
git clone --recurse-submodules https://github.com/thomas-roos/yocto_firecracker.git
```

# build
```
cd yocto_firecracker
source poky/oe-init-build-env build
bitbake core-image-minimal
```

# check kvm permissions
https://wiki.yoctoproject.org/wiki/Running_an_x86_Yocto_Linux_image_under_QEMU_KVM
```
sudo addgroup --system vhost-net
sudo adduser $USER vhost-net
sudo chown root:vhost-net /dev/vhost-net
sudo chmod 0660 /dev/vhost-net
```

# run qemu
```
runqemu nographic kvm-vhost
```

# check inside of qemu that a /dev/kvm exists with correct permissions
```
root@qemux86-64:~# ls -la /dev/kvm
crw-rw-rw-    1 root     kvm        10, 232 Feb  2 11:10 /dev/kvm
```

# run ptests / basic test

```
ptest-runner
```
```
Ubuntu 18.04.5 LTS ubuntu-fc-uvm ttyS0

ubuntu-fc-uvm login: root (automatic login)

Last login: Wed Aug 18 11:44:50 UTC 2021 on ttyS0
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.14.174 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
root@ubuntu-fc-uvm:~# 
```
Hint: "strg+alt c" and then "quit" will end qemu

# Links
https://www.linux-kvm.org/page/Nested_Guests

https://wiki.yoctoproject.org/wiki/Running_an_x86_Yocto_Linux_image_under_QEMU_KVM

https://wiki.yoctoproject.org/wiki/How_to_enable_KVM_for_Poky_qemu

https://wiki.yoctoproject.org/wiki/Running_an_x86_Yocto_Linux_image_under_QEMU_KVM

https://docs.fedoraproject.org/en-US/quick-docs/using-nested-virtualization-in-kvm/

https://kashyapc.wordpress.com/2012/01/14/nested-virtualization-with-kvm-intel/
