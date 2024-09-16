
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

### Examples of hypervisors 
#### Hyper-V

- Microsoft's take on virtualization is Hyper-V, which comes in several forms. 
- Traditionally, Hyper-V is a full operating system (for example, Hyper-V Server 2019), which is a **Type 1 Hypervisor**.
- However, the Hyper-V service can be installed on Windows servers and desktops as a role/feature (such as Pro and Enterprise editions). 
- If installed, the Hyper-V service manages the Windows OS, giving the illusion that it is a type 2 hypervisor, where, in fact, the Windows desktop is managed by Hyper-V from there on.
#### VirtualBox

Oracle's VirtualBox is a popular choice among Hypervisors due to its open-source, free, and cross-platform nature. This is a **Type 2 Hypervisor**
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

Adversaries and APT groups are increasingly focusing their efforts on Hypervisors because they form the backbone of modern computing today. For example, why ransomware one host when you can ransomware a Hypervisor and take down n+1 systems?
### how hardware components are virtualized.

#### CPU & Memory Virtualization

Hypervisors use virtual CPUs (vCPUs) to virtualize the CPU, which are mapped to the cores available on the physical CPU.
In modern Hypervisors, a vCPU is not permanently assigned to a core on the physical  CPU. Instead, the Hypervisor will take the instructions coming from the VM and spread these across the physical cores as needed.
#### Storage  Virtualization

Known as a virtual storage device, a Hypervisor uses virtual disks to create the illusion of a virtual machine having its own physical storage drive. When, in fact, it is sharing the host's physical storage device (such as a hard drive).

Each Hypervisor uses its own format to denote a virtual storage device. For example, VMware uses "VMDK",  whereas VirtualBox uses "VDI".
#### Network Virtualization

Much like CPU, RAM, and storage, Hypervisors abstract the physical networking devices of the host machine to the guest virtual machines. This abstraction is achieved by using Virtual Network Interfaces (vNICs). These vNICs work just like a network adapter and handle data, obtain an IP address and are assigned a MAC address.

Guest virtual machines can have multiple vNICs attached (for example, operating on different subnets simultaneously) while one physical network adapter on the host is used.
There are multiple types of networking with virtual machines namely 

| **Type**  | **Description**                                                                                                                                                                       |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Bridged   | This type allows for a VM to appear as if it is another device on the host network. For example, the guest VM will obtain an IP address on the same network as the host.              |
| NAT       | With this type, all guest network activity appears as if it originates from the host.                                                                                                 |
| Host-only | Host-only will only allow the guest to be accessible from the host itself.                                                                                                            |
| Specific  | The "specific" type allows you to manually assign what vNIC is used by the guest. Hypervisors allow you to create vNICs, where you can specify what subnet and address range is used. |

### Paravirtualisation

Unlike full virtualisation, guest virtual machines using paravirtualisation are aware that they are operating on virtualized hardware. (the guest knows that the CPU, RAM, storage and network are virtualized.) This allows the guest virtual machine to make direct calls to the Hypervisor.

Hypervisors such as KVM use a combination of full virtualisation and paravirtualisation to achieve its performance. For example, KVM uses paravirtualisation for I/O (disk read/writes and network activity) for its performance increase whilst using full virtualisation for components such as CPU & RAM for compatibility.
### Nested Virtualisation

Nested virtualisation allows for virtual machines within virtual machines. For example, running VirtualBox within a guest. This is achieved by using hardware-supported virtualisation such as Intel VT-x or AMD-V. These technologies are features within the CPU that manage virtual machine operations directly rather than using the Hypervisor as the middle-person.
The use of Intel VT-x and AMD-V improves performance because the instructions are handled on the hardware directly rather than through software.
### References

[TryHackMe | Hypervisor Internals](https://tryhackme.com/r/room/hypervisorinternals)