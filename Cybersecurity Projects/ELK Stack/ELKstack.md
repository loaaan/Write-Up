status: in progress

# ELK stack

## What is ELK stack? 

>Is an acronym used to describe a stack that compromises three projects: Elastic Search, Logstash and Kibana.

The E stands for `elasticsearch` which is a distributed search and analytics engine, allows you to store, search and analyze large amounts of data quickly. 
The L stands for `logstash`, this is an open-source data processing pipeline tool, it collects, process and forwards log data.
The K stands for `kibana`, this is a data visualization and exploration tool.

So basically `logstash` works as a delivery guy on his scooter, delivering food made by the kitchen of many restaurants (`elasticsearch`) you tell them what you want and they can prepare it really quick but it is only accessible through the desktop, which is `kibana`. Or something like that.

## Objective

Learn how to set up and configure ELK stash in the cloud, along with attacks, detection and investigation. 
Learn how to create alerts and dashboards and integrate a ticketing system.

## Hands On
So now that I have a bigger picture and better understanding, I will start the process documentation.


## Logical Diagram
The first step is creating a logical diagram. I find it funny to create it and it also allows me to visualize how the connection will work, cause sometimes it can get very abstract.

![logicaldia](./img/logicaldiagram.png)

With that information we can go ahead and start the machines on the cloud and install the programs. 

## VPC creation

![vpc](./img/vpc.png)

> unlike aws, I find VULTR very easy to use and configure

once that I easily created the VPC 2.0 in dallas, I will go ahead and spin up an ubuntu server

## ubuntu server creation

![vpc2](./img/vpc2.png)
![vpc3](./img/vpc3.png)

Pretty straight forward process, it will ask us about how many storage do you want, the version of the Virtual Machine _i chose ubuntu 24.04_. 

![server1](./img/server1.png)

Once the server is up, we can _for practicity_ ssh from our machine to the server in the cloud.

![server2](./img/server2.png)

*******

## Elasticsearch installation

Once that I spined the server up, now i will install elasticsearch using `wget` , then `dpkg -i` to install the program and save the important information we are presented to.

![server22](./img/server22.png)

I need to configure elasticsearch service that I just installed with `nano elasticsearch.yml` and enabling the network connection with the IP of the machine and selecting the port. After that we can activate the elasticsearch service.

![elas](./img/elas.png)

## Kibana installation

The next program to be installed is Kibana, just like the other one, we will go to the download page and use wget on the cloud server.

![kib](./img/kib.png)

after that we need to configure kibana too. to do so we'll use `nano /etc/kibana/kibana.yml/` and enable server port and changing server host to our public IP

![kib1](./img/kib1.png)



I needed to make some settings on the firewall to make my set up secure (only i can access) and we need to set up the VM as well. 

![fw](./img/fw.png)

finally we can access to elasticsearch which will ask us for the enrollment token

> these steps are quite tedious since it is only connecting one program to the other one

we will generate keys from one program to connect to the other one to be able to configure and set up alerts

![kibset](./img/kibset.png)

once our kibana service is set up, we can proceed with the next step

*****
## Windows Server installation
### (with RDP exposed to the internet)

On vultr I went ahead and deployed another server with windows the cheapest option posible

![winserv](./img/wserv.png)

also, I did notice that, since this machine will be exposed, we don't want to have that open access to our vpc, I will not use a VPC in that machine, so I modified the logical diagram.

![logical2](./img/logical2.png)

Once is installed, we can connect to the server with our computer through RDP

![winserv1](./img/wset1.png)

## Elastic agent and Fleet Server

The next step is set up the elastic agent and the fleet server, but what are these things? 
Basically, the agent is the one who actually collects the information from the system, and the fleet server is the centralized system to manage the agents (coordinates elastic agents / collects data)

![fleet](./img/fleetconf.png)

I need to deploy another server for it to be the fleet server (the remote control for the agents), ubuntu, inside of my vpc 2.0.

![fleetc](./img/fleetconf1.png)

On the elastic server i found the instructions to add an agent

![fleetc1](./img/fleetconf2.png)

I made the necessary adjustments for the firewall between my servers.

![fleetfw](./img/fleetfw.png)
![fleetfw2](./img/fleetfw2.png)

Also, i did some adjustments on the configuration command the elastic server is giving me, since the port we are going to use is 8220 instead of 443, and since it is a self signed certificate I needed to specify the flag `--insecure`

![fleetc3](./img/fleetconf3.png)
![fleetc4](./img/fleetconf4.png)

and after the installation is over in the windows server, i can verify the agent on the elastic server

![fleetc5](./img/fleetconf5.png)

*************

## Sysmon

### What is it? 

is a windows system utility used for monitoring and logging system activity in detail. 
runs as a background service and logs key system events, it then can be captured in the windows event log and analyzed for suspiscious activity 
-detailed information like: process creation, network connections, file creation, registry changes, dll loading, ect. 

### Installation
i connected to the widnows server through RDP and downloaded sysmon and sysmon olaf config
![sysmon1](./img/sysmon1.png)
> what is sysmon olaf config?
> since sysmon is very powerfull and the PCs from nowadays does A LOT of things all the time to be able to function, it can log a lot of information we dont need, so in this way sysmon only tracks the really important stuff at the moment we investigate threats or forensics.


![sysmon](./img/sysmon.png)
![sysmon2](./img/sysmon2.png)
![sysmon3](./img/sysmon3.png)
![sysmon4](./img/sysmon4.png)

so now i just installed a new service, configurated in my Windows server, now i have to link that log data my centralized system, elastic search

*******

### Elasticsearch ingest data

going to the elasticsearch GUI server (on port 5601), i need to add an integration

![elasticsearch](./img/elasticSearch.png)

and i will create 2 policies; 1 for sysmon that i just installed, and the other one for windows defender

> why from windows defender? 
> sysmon focuses on detailed system events and windows defender on malware detection and security threats. so if windows defender logs a threat, with sysmon we will have details of the environment at that moment and we can correlate information

>basically for better detection, correlation and response to potential security incidents. 


![elasticsearch1](./img/elasticSearch1.png)
![elasticsearch2](./img/elasticSearch2.png)
![elasticsearch3](./img/elasticSearch3.png)
![elasticsearch4](./img/elasticSearch4.png)

once that is installed and configured, i made sure the firewall was correctly configurated to accept communication through the port that im using to ingest the data to (9200)

after that i can make sure it's working after restarting elastic service on the windows server and verifying something is happenning with the cpu and memory usage

![elasticsearch5](./img/elasticSearch5.png)

**********

## Brute force attack 

so what is it? so just like with the first 2 words of the name of the attack you can imagine what is it about

the attacker attempts to guess a password, encryption key or other secret information by trying all possible combinations until the correct one is found

theres actually different types of brute force attacks like: 
- simple brute force (e.g. aaaa, aaab, aaac, etc.)
- dictionary attack (pre-determined list of likely passwords)
- credential stuffing (likst of known username-password combinations like from previous data breahes)
- hybrid attack (combination of a dictionary attack and simple brute force)

i will simulate a brute force attack, that means that i will be able to know this kind of attack from both perspectives and analyze best way to defense against this attack

## Installing SSH server

after spin up a really simple ubuntu server, we can connect to it through Powershell

![sshserv](./img/sshserv.png)


once in i update the repositories

```sh
apt-get update && apt-get upgrade -y
```

the next step is installing elastic agent to collect the logs and send them over to our ELK server

## Elastic agent in linux server

So... what will the agent collect from the linux server? 

The good thing about the agent is that i can collect specific data based on our configuration; for example, we can collect system metrics (CPU usage, memory utilization, network traffic, etc), application logs, system logs (kernel messages, authentication logs, security events, etc.) or custom metrics.

### Agent installation in linux

THe first thing to do is go to our gui elk web server and add another agent policy

![linuxagent](./img/LinuxAgent.png)

it is (sorprisingly) not that hard since elk give us all that we need, we just need to tweak some things from our side since we are not configuring any key. 

![linuxagent1](./img/LinuxAgent1.png)

it gave me an error at the moment of installation (thanks for the troubleshooting practive) about the certificate, this is because we're using a self signed certificate (generated by the entity that created the agent). It doesn't represent an issue for me since I'm making this only as a project in a home lab environment but is very needed when setting up a production environment. 

![linuxagent2](./img/LinuxAgent2.png)

>so i just needed to add the `--insecure` flag

after that it seems that the installation went through, i will wait a bit and check in Kibana if we are receiving the logs from our linux machine. 

![linuxagent3](./img/LinuxAgent3.png)

And we got it working!!

So now that i set up two vm's to send logs to my SIEM server, i will create the alerts in Kibana.


## Kibana dashboards

im very excited for this part of the project cause i really be feeling like a hacker man

### Linux authentication map

![AlertsKib](./img/AlertsKib.png)

first i need to identify the query that i want to visualize, in this case, i will first set up the authentication events from the linux server and create a rule. 

![AlertsKib1](./img/AlertsKib1.png)

and, since i want to visualize a world map that let me know from where the attacks are coming from, i added a layer where the logs will give me the iso code from the country and, in order to visualize it, i need to collect that information from the authentication events

> why the hell do i need to select the iso code of the country instead of the name? 
> consistency and standardization, codes are numbers, and these are universally recognized to ensure consistent representation of countries, so that way we have data accuracy, and also flexibility


![AlertsKib2](./img/AlertsKib2.png)

good so i just created a map that queries the failed authentication logs to my linux server, obviously, we are interested also in the successful ones, so it's the same query but with the `accepted` field instead of `failed`.

```
system.auth.ssh.event : * and agent.name: MYDFIR-Linux-loaan and system.auth.ssh.event: Failed
```
![AlertsKib3](./img/AlertsKib3.png)

### Windows authentication Map

so, just like the linux one, i needed to identify the query that gave me the information i wanted, in this case, failed authentication attempts, and it was easy to find since most of the logs were about that

![rdpKib](./img/rdpKib.png)

```
event.code: 4625 and agent.name: MYDFIR-WIN-loaan and user.name: Administrator
```

this was a lil bit different since windows manage this kind of events different, the event code for the successfull authentication changes, a quick search let me know what i wanted [here](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4624)

![rdpKib1](./img/rdpKib1.png)

after that i created another 2 maps from the windows server

![rdpKib6](./img/rdpKib6.png)

also, i added another dashboard but it was not a map, but a list from the countries that we are getting 'attacks' from

![rdpKib3](./img/rdpKib3.png)

![rdpKib7](./img/rdpKib7.png)

## Rule Creation

ok, this looks pretty ngl, but the main thing that the SIEM exists is that we need to know any time we are getting suspicious activity so now, i created a rule to be able to catch these events for both servers.

![rule](./img/rdpKib2.png)

![rule1](./img/rdpKib5.png)

and the rule is created

![rule2](./img/rdpKib4.png)

### Remote Desktop Protocol (RDP)

i have talk about how i set up the windows machine with the RDP enabled, but what is it? 
Its a technology that allows access and control a remote computer from another device over a network, event though, it's quite secure the connection since its encrypted, we left it open to the world to see who is trying to connect, usually it's an automated thing for the attackers. 

**********

### Command and Control (C2)

the next part of the project is the creation of the C2 server, just like i planned it on the logical diagram

but what is it? 
is a term used in infosec and cyberwarfare to describe the infraestructure and techniques used by attackers to communicate with and control compromised systems

its composed by the command server (receives commands from the attcaker and sends instructions to compromised systems), botnet, communication channels (like http, https, DNS queries or specialized protocols) and obfuscation and evation techniques.


#### Attack Diagram

#### Mythic Installation

#### Agent Installation

#### Attack Simulation

### Create alert and dashboard 

*******

### Ticketing System

