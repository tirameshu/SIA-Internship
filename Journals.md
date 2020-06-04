# 3 June

## Plan 
- Use variable names router_1, router_2 to refer to the routers, without specifying their IP addresses first.
- Use iso_config (Ansible network module) on routers


## Questions
- Are the files /etc/hosts, /etc/ansible/* for public usage? Is is safe for me to edit?
	- Let Abhay know before editing

- I am unable to connect to CheckPoint VPN, I also forgot what it is used for.
	- Office work, connect with PIN + RSA

- CCNA lab shared with me, not sure how to derive specific IP addresses of devices from the range of IP addresses given as 192.168.11.0/24 Transit LAN.
	- Abhay will explain lab to me.

- Is it possible/ advisable for me to log into the company email from phone? Exchange server says unable to verify account.
	- MDM, will set up.

- Are the files I'm uploading onto BitBucket are ok, do they contain sensitive info, etc..

# 4 June

## Accomplishments
1. Based on GNS3 tutorials and Cisco Basic Router Configuration Guide, added some `ios_config` commands to playbook.
2. Learnt about EVE Topology Lab usage.
    - ping the desired addr for VLAN. If unreachable --> not taken.
3. Learnt about switches and VLANs.
    - VLAN
        - a subnet in a switch, which by default is just 1 subnet
    - Layer 2 switches:
        - used at Access layer --> directly connected to PC/ server, which have their own IP addresses
        - packets are sent to a specific switch port (*layer 2 ports*) based on destination *MAC addresses*
        - can assign VLANs to specific switch ports, which could be in different layer 3 subnets
            - so different layer 3 networks can share the same layer 2 infrastructure
            - however, the layer 3 subnets are **assigned by layer 3 switches**, and the only IP addresses associated with layer 2 swtiches are the addresses of the devices in their VLAN
        - uses MAC address
    - Layer 3 switches:
        - a switch that can perform routing
        - has both *interface VLAN* as well as normal VLAN
            - basically a routable interface, each can be assigned an IP address
            - reachable over the network
        - requires a default gateway to connect to remote subnets
4. General connection procedures
    - PC -> switch:
        - ssh into switch with switch ip/ hostname
        - run commands
    - Ansible -> switch (in same subnet):
        - Ansible will ssh into switch instead
            - if switch is in the same subnet, no routing is needed
        - switch IP would be described in host file
        - run commands with Playbook
- EVE usage
    - only use 1 "Network" component for connection
    
## Questions
### Q1: Lab usage
A1: As above

### Q2: Do layer 2 switches only an IP address for management purpose, and does not associate any IP address with each port?
- So if a layer 2 switch has multiple VLANs, each VLAN is not accessible over the network (because they have no IP address), but can be accessed via layer 3 switch VLAN interfaces?
- Does each device in the VLAN under a layer 2 switch have their own IP address, under the same subnet? (Refer to https://documentation.meraki.com/MS/Layer_3_Switching/Layer_3_vs_Layer_2_Switching)

A2:

### Q3: Is layer 2 switch sufficient for Ansible -> switch connection, if they are in the same subnet?
- Or if they are not in the same subnet, then Ansible cannot directly connect to a layer 2 switch?
A3:

### Q4: Are the files I'm uploading onto BitBucket are ok, do they contain sensitive info, etc..

## To Do
- More research on using EVE
- Reserve some free IP addresses for my own use and try to connect to them

# 5 June

## Questions
### Q1: How to access the SharePoint spreadsheet of IP addresses
