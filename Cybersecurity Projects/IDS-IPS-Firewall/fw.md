# Firewall and IDS/IPS monitoring

## Objective
Set up a firewall and Intrusion Detection/Prevention System in my home lab environment, monitor the traffic and analyze security alerts. 

## Technologies 
all the technologies i used for this home lab is free and open-source 

#### pfSense
works as firewall and router software, provides advanced networking features and security capabilities 
it has many key features like firewall, router, VPN, QoS (Quality of Services ensure that critical applications receive bandwith required), Dynamic DNS, Content Filtering, IDP (monitors suspicious activity and can take action to prevent attacks) and Load Balancing. 


#### Snort
powerfull network intrusion detection system, its designed to monitor network traffic for suspicious activity and provide alerts when potential threats are detected
it also has many features like packet inspection, rule-based detection (flexible rule engine to define detection rules), signatures (comes with a large database of pre-defined signatures for known attacks), logging and inline mode (to be used like IPS instead of only IDS)

#### Wireshark
network protocol analyzer. captures and analyze network traffic so we can inspect packets and understand how network protocols are working. 
its has many features like packet capture, protocol analysis, filtering, timelines, export (we can export the captured packets to various formats like .csv or .xml) and plugins (extend its functionality, like VoIP analysis or wireless network analysis).

#### Wazuh
security monitoring platform designed to protect endpoints, networks and cloud environments. It offers a combination of Security Information and Event Management (SIEM) and Extended Detection and Response (XDR)
it has a lot of features, like endpoint security (configuration assesment, malware detection, file integrity monitoring), threat intelligence (threat hunting, vulnerability detection, log data analysis), security operations (incident response, regulatory complience, IT hygiene) and cloud security (container security, posture management and workload protection)



ffiiiiiiiuuuuu, let's work.

## Logical diagram