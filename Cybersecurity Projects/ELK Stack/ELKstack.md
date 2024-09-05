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