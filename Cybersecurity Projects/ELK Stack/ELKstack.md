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
