# Homelab Setup

## Objective

The Homelab Setup project is aimed to establish a controlled environment for simulating and detecting cyber attacks. I set up a home lab using two virtual machines using VMWare as the hypervisor. My host machine is a MacBook Pro M2 Max. I set up these two VMs for the purpose of conducting tests on cybersecurity tools. VMs also allow the ability conduct these tests in a controlled environment without causing harm to the host machine.

The primary focus was to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies. 

### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.
- Advanced understanding of setting up virtual machine hypervisor
- Installation of Microsoft Windows 11 client and Kali Linux client
- Network configuration to enable the virtual machines to communicate

### Tools Used

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps
<img width="768" alt="image" src="https://github.com/user-attachments/assets/5d058a4f-3784-4298-9337-c96dcbb0e91b" />

In the screenshot above, I show the two VMs I created and have hosted in VMWare. I installed Kali Linux for ARM architectures as well as Windows 11 ARM since the host machine I am on is an Apple MacBook Pro with the M2 Max CPU. Each of the VMs are configured with 4GB of RAM, 50GB HDD space, as well as 2 CPUs (processor cores). The two VMs are on the same network as administered by VMWare & the host machine through NAT. This way, both VMs can see the other VM. The IP addresses of both VMs will need to be statically assigned so that each VM will be able to see the other one.

<img width="768" alt="image" src="https://github.com/user-attachments/assets/fa283314-1bfe-4920-885b-64ea3354d7ee" />

In the Windows 11 VM, I statically assign the IP address to 192.168.20.10. The subnet mask defaults to 255.255.255.0.

<img width="768" alt="image" src="https://github.com/user-attachments/assets/8e1fa3f0-1629-4fb6-b188-9dc0f93c5fe1" />

I open the Command Prompt and type ipconfig to bring up the Windows IP configuration. This is to verify that the IPv4 address has been changed.

<img width="768" alt="image" src="https://github.com/user-attachments/assets/ef31e6c5-622f-4e3e-be5d-a43205ad7890" />

In the Kali Linux VM, I statically assign the IP address to 192.168.20.11. The subnet mask defaults to 255.255.255.0.

<img width="768" alt="image" src="https://github.com/user-attachments/assets/29a72328-833b-44fc-ad2e-41c574f2a34f" />

I open the Terminal and type ifconfig to bring up the internet configuration. This is to verify that the IPv4 address has been changed. Next, I ping the Windows VM by typing ping 192.168.20.10 but see that it will not work since Windows Firewall is blocking the incoming traffic.

<img width="768" alt="image" src="https://github.com/user-attachments/assets/7c230581-9ccb-4e17-8930-4f8a7b99a219" />

In the Windows VM, I type ping 192.168.20.11 in Command Prompt to ping the Kali Linux VM. This shows activity in Command Prompt and verifies that both VMs can communicate with one another. 
