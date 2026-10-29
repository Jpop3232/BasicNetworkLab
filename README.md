# Basic Network Lab
## Purpose of this Project
This project was created to simulate a very simple network of two servers, and to showcase how to troubleshoot and address any issues that may occur that prevents servers from connecting within a network.
## Setup 
This project is ran using two Ubuntu VMs from Oracle VirtualBox Manager. For the sake of simplicity they are on a shared NAT network (jordNetwork) with a custom IP address (192.167.100.0). The VMs used are labled CMSC430 (197.167.100.3) and secondVM(197.167.100.3). Both VMs need the applications net-tools and traceroute installed in order preform this project. An optional application is gufw, it is not needed to run this project but if you want to manage ufw with a gui then it is reccomended.  
## Testing Connectivity 
Two methods that can used to test the connectivity of the servers within the network is to PING or to Traceroute. 
### Ping
Ping measures the amount of time it takes for the smallest packet of data to get from one server to another, measured in milisecods. The lower the ping, the better the latency and the faster the connection. Getting as close to zero is the goal, but even in the 20-50 ms range is servisable in daily activities such as streaming and calls. To test ping, simply open the command terminal and type "ping *IP address of the server you want to connect to*" demonstrated on the servers below:  
<img width="637" height="327" alt="image" src="https://github.com/user-attachments/assets/beaa2c04-a12d-4997-afac-88009fb38dbb" />  
  _server cmsc430 command "ping 192.167.100.4"_  
  <img width="641" height="285" alt="image" src="https://github.com/user-attachments/assets/b09ebbb2-4a7e-45ed-8b6d-5f1b2f3b657e" />  
  _server secondVM command "ping 192.167.100.3"_  
### Traceroute
Traceroute is an application that records the path that packages take to get from one server to the next. It shows what the set maximum amount of hops are allowed within the network, and the size of the packets sent out from a server. When a server requests to document the path to another server from traceroute, it will display each hop, the IP address of where the packet is after each hop, and the amount of time it takes for that packet to make a round trip in ms, with three packets sent to give an average round-trip time. To see the traceroute from one server to another, input the command "traceroute *IP address/FQDNS of the server you want to connect to*" demonstrated below:  
<img width="747" height="75" alt="image" src="https://github.com/user-attachments/assets/f0f057a2-6866-4a50-8105-d102a907d133" />  
_server CMSC430 command "traceroute 192.167.100.4"_  
### SS
The ss, short for server socket, command in linux allows the user of a server to see the conditions of the network sockets on said server. Typing in ss shows all sockets and their conditions, but additional commands can be added to show more specific data, such as only showing TCP or UDP sockets, showing only listening sockets, and so on. The ss command can be helpful in showing where in a server that bottlenecks are occuring by checking unusual data queues in reciving and sending. Below is the summary statistics of both servers:  
<img width="586" height="281" alt="image" src="https://github.com/user-attachments/assets/add000e1-1fb0-4d2c-99ef-0a9ef54e454c" />  
_secondVM ss summary statistics_  
<img width="597" height="237" alt="image" src="https://github.com/user-attachments/assets/f067ac57-1901-4b49-b09f-e9ba03f0dd8d" />  
_cmsc430 summary statsitics_  
## Troubleshooting
### Example #1
For this example I disconnected the server cmsc430 from the network. Whenever secondVM tried to ping cmsc430, it showed that secondVM could send out data, but that data was not recived by the host, three errors also occured. When running the traceroute it showed a failed hop with the error code !H, which means that the host is unreachable. In the case that I was not aware that server cmsc430 was offline, using the information gained from the ping and traceroute (information was successfully being sent but not recived and the host is unreachable error code) I could determine that the problem was not with secondVM and was instead an issue with cmsc430. Knowing that, I can focus on trying to see what is wrong with cmsc430 instead of spending time trying to fix secondVM which has no issues.   
<img width="896" height="302" alt="image" src="https://github.com/user-attachments/assets/8d1829ed-6400-4125-a5ee-1fc5d9f7031d" />  
_attempted ping and traceroute from secondVM_  
### Example #2
For this example I set the firewall on secondVM to deny both incoming and outgoing data, which leads to a signfigantly different message, ping shows data was sent out but not recived and the traceroute attempt only shows a message stating that there is no permission to send data. In the case that we were not aware of this strict firewall being up, knowing that data can not even be sent indicates that the issue is on secondVM's side, and knowing that the ping attempted to send data shows that there is network connectivity. 
<img width="757" height="240" alt="image" src="https://github.com/user-attachments/assets/f723ab02-d67e-4dd7-882c-b5f8082e9f36" />  
_attempted ping and traceroute from secondVM_  
### Example #3
For this example I disconnected secondVM from the network. Attempts to ping and traceroute gave back the statement "connect: network is unreachable". This error message is simple and straightforward and tells me that it is secondVM that is disconnected from the network, narrowing down the checks I need to make.  
<img width="800" height="152" alt="image" src="https://github.com/user-attachments/assets/49fd65b1-8c2d-49c5-918e-c4280a31383f" />  
_attempted ping and traceroute from secondVM_
# Takeaway
Ping, traceroute, and the ss comand are all extremely helpful in acting as a diagnostic tool for network connectivity issues. Knowing how to use them can prevent waisting hours attempting solutions that will not fix the existing problem. They make troubleshooting signifigantly simpler and intutive.
