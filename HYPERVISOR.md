[[VIRTUALIZATION]] [[OS]]

>[!info] Definition
>A **hypervisor**, also know as a **virtual machine monitor (VMM)** or **virtualizer**, is a type of computer software, firmware or hardware that creates and runs *virtual machines*.

The computer on which a hypervisor runs one or more virtual machines is called a **host machine**, and each virtual machine is called a **guest machine**. The hypervisor presents the guest operating systems with a virtual operating platform and manages the execution of the guest operating systems. Unlike an emulator, the guest exectutes most instructions on the native hardware.
Multiple instances of a variety of operating systems may share the virtualized hardware resources: for example, Linux, Windows and MacOS instances can all run on a single physucal **x86** machine. This contrast with operating-system-level virtualization, where all instances (usually called *containers*) must share a single kernel, though the guest operating systems can differ in user space, such as different Linux distributions with the same kernel.

Some literature, especially in microkernel contexts, makes a distinction between *hypervisor* and *virtual machine monitor (VMM)*. There, both components form the overall *virtualization stack* of certain system. *Hypervisor* refers to kernel-space functionality and VMM to user-space functionality. Specifically in these contexts, a hypervisors is a microkernel implementing virtualization infrastucture that must run in kernel-space for technical reasons, such as Intel VMX. Microkernels implementing virtualization mechanisms are also referred to as *microhypervisor*. Applying this terminology to Linux, *KVM is a hypervisor* and *QEMU or Cloud Hypervisor are VMMs* utilizing KVM as hypervisor.
### Classification
In his 1973 thesis, "Architectual Principles for Virtual Computer Systems", Robert P. Goldberg classified two types of hypervisor:
#### Type-1, native or bare-metal hypervisors
These hypervisors run directly on the host's hardware to control the hardware and to manage guest operation systems. For this reason, they are sometimes called bare-metal hypervisors. Examples of Type-1 hypervisors include Hyper-V, Xen and VMwate ESXi.
#### Type-2 or hosted hypervisors
These hypervisors run on a conventional operating system (OS) just as other computer programs do. A virtual machine monitor runs as a process on the host, such as VirtualBox. Type-2 hypervisors abstract guest operating systems from the host operating system, effectively creating an isolated system that can be interacted with by the host. Examples of Type-2 hypervisors include VirtualBox and VMware Workstation.
#### Overall
The distinction between these two types is not always clear. For instance, KVM and bhyve are kernel models that effectively convert the host operating system to a type-1 hypervisor.

![[Pasted image 20250120234621.png]]

