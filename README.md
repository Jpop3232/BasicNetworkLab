# BasicNetworkLab
This project configures two VMs to the same internal network and documents basic TCP/IP and DNS issues and simple troubleshooting.
To set up this project ensure that the two VMs are on the same NAT network. For this project I made the NAT network jordNetwork, while 
Oracle VirtualBox has its own premade IPv4 prefix, I wanted to have a bit of fun and make my own IPv4 prefix which is just 192.167.100.0
applications needed to be installed on both VMs for this project: 
net-tools
traceroute
Also ensure that net-tools and traceroute is installed onto both VMs.
Configuration for VM CMSC430:
<img width="1367" height="727" alt="image" src="https://github.com/user-attachments/assets/0f7bc8af-93c7-4842-a565-98abaefc753e" />
Configuration for secondVM: 
<img width="815" height="555" alt="image" src="https://github.com/user-attachments/assets/25cd8d7d-848e-40d8-8a08-cee5cab313c1" />
To show that they are connected to the same network I am going to ping each machine from its sister machine (i.e. ping secondVM from CMSC430 and ping CMSC430 from secondVM). It may be redundant to ping on both VMs, but I'm having fun playing with them so no harm in the redundancy. I let both machines ping each other for ~5 seconds just to give a brief demonstration. 
CMSC430 pinging secondVM:
<img width="1122" height="491" alt="image" src="https://github.com/user-attachments/assets/b0f75042-eb55-4c89-bdbf-d7d7d9a950e8" />
secondVM pinging CMSC430:
<img width="652" height="310" alt="image" src="https://github.com/user-attachments/assets/c5a7f1ee-6a01-460f-9473-cd9ff1abbaf1" />
Now obviously there were no issues with the two VMs pinging each other. However there can be issues that come up that prevent two servers from connecting, one of them being that a server has a firewall up. For the sake of demonstration I will put up a firewall on secondVM and try to ping CMSC430 from it.
I changed the rules of the ufw on secondVM to deny both incoming and outgoing traffic, I also closed down its ports for both 22 and 22/tcp
<img width="627" height="287" alt="image" src="https://github.com/user-attachments/assets/5b66fc2e-4f2e-407d-a08d-ac0fa3f796dd" />
Now, whenever I try to ping CMSC430 this is what occurs: 
<img width="717" height="127" alt="image" src="https://github.com/user-attachments/assets/5e9f2170-d66d-4944-89d1-9bdbe3c5b389" />
Due to the firewall I put in place on secondVM, any information that CMSC430 puts out is not reccived. A simple fix would be to just disable the firewall. 
Example of the firewall being disabled and what ping looks like: 
<img width="637" height="432" alt="image" src="https://github.com/user-attachments/assets/47a86f97-a342-4502-bc8f-68ac30c1bd2b" />
Now I was going to use traceroute to check the path that each VM took to reach each other, but I ran into an issue:
<img width="1346" height="175" alt="image" src="https://github.com/user-attachments/assets/c8e48686-1372-4729-90b1-a512d701d329" />
and that was because I did not create a domain name for my VMs and I conflated the host name with a domain name, which was causing confusion in my commands.
