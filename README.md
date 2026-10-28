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
Example of firewall being disabled and what ping looks like: 
<img width="637" height="432" alt="image" src="https://github.com/user-attachments/assets/47a86f97-a342-4502-bc8f-68ac30c1bd2b" />
Now, I'll use traceroute to examine the paths both servers take to connect to each other through the network.
path secondVM takes to get to CMSC430: 
<img width="776" height="120" alt="image" src="https://github.com/user-attachments/assets/362f0e85-5b6d-45a7-96b5-b504bf334006" />
path CMSC430 takes to get to secondVM: 
<img width="750" height="95" alt="image" src="https://github.com/user-attachments/assets/4e33d9dc-4358-4ac7-8ee9-3a0d60fd4cb0" />
Seen here, both routes only take one hop, this makes sense as there is only one other server in the network, so there is only one possible hop that can be preformed. Even in simple networks like this one issues can show up such a strict firewall preventing one server accessing another, leading to continually trying to make a hop that fails. Which looks like stars as there is no information about the hops to report on.
<img width="757" height="712" alt="image" src="https://github.com/user-attachments/assets/dd89fdad-f39e-4b19-bd3f-737735cdc1c3" />
Which, like the ping issue can be resolved by either disabling the firewall.
Another issue that can prevent two servers from communicating in a network is the host that the server is trying to reach is unreachable. This is demonstrated when I run traceroute in secondVM to CMSC430, the hop gives me a !H, indicating the host is unreachable. This makes sense as I have powered off CMSC430. 
<img width="772" height="182" alt="image" src="https://github.com/user-attachments/assets/39c8a9bd-a876-4c60-8497-8ac261022d3a" />
<img width="867" height="176" alt="image" src="https://github.com/user-attachments/assets/46770d01-f6e6-4ab7-9a1e-8b4aab86caca" />
While an unreachable host can be caused by a number of issues, in this instance the solution was as simple as turning on CMSC430
(Comparsion of unreachable host to reachable host)
<img width="871" height="177" alt="image" src="https://github.com/user-attachments/assets/4ade1cae-91da-4d0f-95b5-d827425710c6" />



