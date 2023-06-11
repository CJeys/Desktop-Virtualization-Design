# Desktop Virtualization Design

# HYPERVISOR
For this solution, Hyper-V Manager will be used.  Hyper-V is a Type 2 hypervisor as the hardware being used is not dedicated to just running virtual machines.  The hypervisor configurations in the four primary computing categories are as follows, based on the situational requirements…

**Processors:** The CPU utilization of the hypervisor is not to exceed 50% of the available CPU
Hard Drives: Storage consumption of total physical storage is not to exceed 30%

**Memory:** The memory will be configured statically, and consumption should not exceed 60%

**Network:** Two separate and isolated virtual networks will be created with pfSense software to keep network traffic segregated. 

In addition to these four areas, **NUMA Spanning** & **Enhanced Session Mode Policy** will be enabled.

# ALLOCATIONS
**Processor Allocations for All VMs:** Each VM is configured with a single core.  In the current lab environment, a single core is more than sufficient in satisfying the situational requirements while not exceeding 50% CPU utilization of the physical server.

**Storage/Hard Drive Allocations for All VMs:** All VMs are configured with a 20GB virtual disk, with an additional 5GB virtual disk attached.  This equates to 125GB of total storage which will prevent the virtual environments from consuming more than 30% of the approx. 447GB of storage, which is the total amount available on the physical server.  

**Memory Allocations:** The VMs memory is calculated to not exceed 60% of the 16GB of total memory available on the physical server.  The VMs combined are configured with approx. 9.6GB of memory.  Each VM’s memory allocation is listed below…

•	ClientA – 1920MB

•	ClientB – 1920MB

•	C01 – 2048MB

•	UBU1 – 2048MB

•	Router – 1664MB

**Network Allocations:** The ClientA and C01 virtual machines contain one virtual network adapter each.  Those adapters are attached to the NetWest virtual switch, and that switch is connected to the Router virtual machine’s ‘NetWest’ virtual network adapter.

The ClientB and UBU1 virtual machines also contain one network adapter each.  Those adapters are attached to the NetEast virtual switch, and that switch is connected to the Router virtual machine’s ‘NetEast’ virtual network adapter.

The Router virtual machine contains two virtual network adapters.  One network adapter is attached to the NetWest network and the other network adapter is attached to the NetEast network.  To isolate the networks, a firewall will be configured blocking traffic from the virtual machines in the NetWest network from communicating with the virtual machines within the NetEast network, and vice versa.

# IMPLEMENTATION
1) Hyper-V Manager needs to be enabled within Windows Features.
2) Hyper-V settings should be setup with the appropriate default storage location for the virtual machines and hard disks along with NUMA Spanning and Enhanced Session Mode Policy enabled.
3) Virtual Switch Manager is used to create the NetEast and NetWest virtual switches
4) Setup ClientA virtual machine with the following…

    •	Windows 10

    •	1 CPU core

    •	1920MB RAM

    •	Primary virtual disk – 20G

    •	Secondary virtual disk – 5G

    •	One network adapter

    •	IP address configured – 172.16.1.2/24

    •	Gateway configured – 172.16.1.1

5) Setup ClientB virtual machine with the following…

    •	Windows 10

    •	1 CPU core

    •	1920MB RAM

    •	Primary virtual disk – 20G

    •	Secondary virtual disk – 5G

    •	One network adapter

    •	IP address configured – 172.16.2.2/24

    •	Gateway configured – 172.16.2.1

6) Setup C01 virtual machine with the following…

    •	CentOS

    •	1 CPU core

    •	2048MB RAM

    •	Primary virtual disk – 20G

    •	Secondary virtual disk – 5G

    •	One network adapter

    •	IP address configured – 172.16.1.3/24

    •	Gateway configured – 172.16.1.1

7) Setup UBU1 virtual machine with the following…

    •	Ubuntu

    •	1 CPU core

    •	2048MB RAM

    •	Primary virtual disk – 20G

    •	Secondary virtual disk – 5G

    •	One network adapter

    •	IP address configured – 172.16.2.3/24

    •	Gateway configured – 172.16.2.1

8) Setup Router virtual machine with the following…

    •	pfSense

    •	1 CPU core

    •	1664MB RAM

    •	Primary virtual disk – 20G

    •	Secondary virtual disk – 5G

    •	Two network adapters

    •	IP address configured for network adapter #1 – 172.16.1.1 

    •	Network adapter #1 connected to NetWest virtual switch

    •	IP address configured for network adapter #2 – 172.16.2.1 

    •	Network adapter #2 connected to NetEast virtual switch

# VIRTUALIZATION BUILD
**STEP 1** 
Hyper-V Manager needs to be enabled within Windows Features.  The screenshot below depicts the settings required to enable Hyper-V Manager
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/f2762693-ec22-4b04-8f7a-c1e562d94caa)

**STEP 2**
Hyper-V settings should be setup with the appropriate default storage location for the virtual machines and hard disks along with NUMA Spanning and Enhanced Session Mode Policy enabled.  The screenshot below depicts where the Hyper-V settings are to be changed.
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/c0e8b2d3-d38f-4478-8dc3-f3d5e729873e)

**STEP 3**
Create internal virtual switches - NetEast 172.16.2.1 & NetWest 172.16.1.1.  The screenshot depicts both virtual switches within Virtual Switch Manager.
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/244a8e06-835b-4514-a825-3a45b079c367)

**STEP 4**
Create the Client A virtual machine with the settings and IP address depicted below.
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/da6f83e0-b653-448e-809f-8be01966f456)
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/b43598c4-a205-40cc-b1ae-ff592f80a39a)

**STEP 5**
Create the ClientB virtual machine with the settings and IP address depicted below.
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/ea3bfe78-562e-4bb3-aa13-a3f31adf9052)
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/3da61a44-e6e1-4f58-ae6b-2a85ba273cf8)

**STEP 6**
Create the C01 virtual machine with the settings and IP address depicted below.
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/9a1f599a-7544-4be8-a607-0a84a917c281)
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/a0220992-ff69-4bf4-b9bc-eba953372b2d)

**STEP 7**
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/0a8843eb-777b-4988-9129-1386ab5fe57a)
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/080cd4d3-2ac1-4316-b236-64e5e63aadc4)

**STEP 8**
Create the Router virtual machine with the settings and IP address depicted below.
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/174b0e86-7dee-4799-aee6-87c2bd9fddb7)
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/3e86506e-bf36-475a-b1fc-3c26284868a9)

**STEP 9**
Create the firewall rules depicted below within pfSense.
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/b14dee49-148f-4c06-a2f8-1f36a9c0729c)
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/eb1808ae-9cb7-4e0c-affe-1425c0082b0f)

**STEP 10**
Enable resource metering and configure counters from within Performance Monitor to track system metrics.
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/b7631fc7-c583-4b8a-93c2-35885bdd08b2)
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/80fd1693-30b7-4a8e-ab87-e0c12d954bd0)
![image](https://github.com/CJeys/Desktop-Virtualization-Design/assets/126273127/225f122a-7f94-4240-9fd4-3eee0684986f)
