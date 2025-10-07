Trying to setup GOAD, running into virtual box linux headers issue
Virtualbox.service - LSB virtualbox linux kernel module
- Use kvm driver based hypervisors
Linux wants drivers to be maintained as part of kernel source tree because drivers are tied to some specific kernel version

KVM is part of source tree, so it never has issues with kernel compatibility but drivers for VirtualBox and VMWare are managed out of tree, so they lag behind and then you update the kernel. Since things changed internally, the driver may or may not work with newer kernel hence the issues