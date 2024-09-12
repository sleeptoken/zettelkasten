
2024-09-12 20:09

Source: #tryhackme 

### Virtualization

Virtualization is the concept of creating multiple virtual environments from shared physical hardware. 
Virtualization allows multiple operating systems to run on a physical device, where the operating systems do not interact or conflict with each other. These multiple environments are known as **Virtual Machines** (VMs).
### Types of Hypervisors

Hypervisors are divided into two categories, which define their position (abstraction) in relation to the hardware of the physical device.

#### Type 1 (Bare metal)

- These types of Hypervisors have direct access to a system's physical hardware,
- Hypervisors like Hyper-V and VMware ESXi are great examples of type 1 Hypervisors. Additionally, type 1 Hypervisors form the backbone of cloud computing.

#### Type 2 (Hosted)

- These Hypervisors, also known as hosted Hypervisors, because they run on top of an existing operating system (such as Windows or Linux)
- Examples of hosted Hypervisors included VirtualBox and VMware Workstation/Player.

Examples - 
#### Hyper-V

- Microsoft's take on virtualization is Hyper-V, which comes in several forms. 
- Traditionally, Hyper-V is a full operating system (for example, Hyper-V Server 2019), which is a **Type 1 Hypervisor**.
- However, the Hyper-V service can be installed on Windows servers and desktops as a role/feature (such as Pro and Enterprise editions). 
- If installed, the Hyper-V service manages the Windows OS, giving the illusion that it is a type 2 hypervisor, where, in fact, the Windows desktop is managed by Hyper-V from there on.
#### VirtualBox

Oracle's VirtualBox is a popular choice among Hypervisors due to its open-source, free, and cross-platform nature. This is a Type 2 Hypervisor
#### VMware ESXi

ESXi is a member of VMware's vSphere suite. This **Type 1** Hypervisor is a popular choice for large-scale virtualisation. Especially because of features such as:

- **Clustering:** ESXi installations on multiple machines can be installed and connected to one another, allowing workloads to be distributed
- **Automation:** ESXi includes a rich API, allowing the automation of various tasks. For example, automatic snapshotting, VM deployment and management.
- **High availability:** In the event of an error, failover tasks such as virtual machine restart and "offloading" are possible within ESXi.
#### VMware Workstation

VMware Workstation is another Hypervisor offering from VMware. Previously requiring the purchase of a license, Broadcom, VMware's owner, made it free for personal use. 
Workstation is considered a **Type 2** (hosted) Hypervisor and is more feature-rich than alternatives such as VirtualBox because of features such as:
- Being able to organize VMs in folders
- Finer controls over network adapters
#### QEMU

QEMU, also known as Quick EMUlator, is another open-source Hypervisor. This Hypervisor is known for its ability to emulate full systems. 
- Additionally, QEMU can be combined with KVM to have near-native performance by executing instructions directly on the host's CPU.  While QEMU has features of other Hypervisors, such as snapshotting, it can be arguably considered less user-friendly since it is used on the CLI.
- However, with that said, front-end interfaces have been created that can be used. 
- These two technologies (QEMU & KVM) are often used in cloud environments due to their performance and compatibility.

### References

[TryHackMe | Hypervisor Internals](https://tryhackme.com/r/room/hypervisorinternals)