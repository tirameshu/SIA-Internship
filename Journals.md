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

## Learnings
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

A2: should be yes

### Q3: Is layer 2 switch sufficient for Ansible -> switch connection, if they are in the same subnet?
- Or if they are not in the same subnet, then Ansible cannot directly connect to a layer 2 switch?
A3: Yes

### Q4: Are the files I'm uploading onto BitBucket are ok, do they contain sensitive info, etc..
A4: Ok

## To Do
- More research on using EVE
- Reserve some free IP addresses for my own use and try to connect to them

# 5 June

## Learnings
- Router Configurations
    - first ping the host to see if router can go thru network
    - then ping router from host to see if can reach
    - set up ssh (rsa fingerprint) before trying to reach
    - write config before logging out
    
    1. `conf t`
    
    2. `hostname [swtich name]`
    
    3. `int vlan [n]`
    
        3.1 `ip addr [addr] [subnet]`
        
        3.2 `no shutdown`
        
    4. `username [username] password [password] privilege [privilege]`
    
        - to facilitate ansible ssh into switch
        
    5. Inbound connections
    
        5.1 `line vty 0 4`
        
            - device can allow 5 simultaneous virtual connections which may be Telnet or SSH
            
        5.2 `transport input all`
        
        5.3 `login local`
        
    6. `ip domain-name ansibleautomation.com`
    
    7. `crypto key generate rsa`
    
- Layer 3
    - needs layer 2 to work
- IP assignment
    - if VLAN has DHCP, it will assign IP
    - if not, have to look for central DHCP (outside subnet, via gateway) to assign
        - can be too expensive to have 1 DHCP per VLAN
- Connection type
    - local: Ansible connects to devices via local modules, not copy modules to device.
    - specific direct connections, eg paramiko: Ansible connects to device directly

## Questions
### Q1: How to access the SharePoint spreadsheet of IP addresses

### Q2: How to access variables outside current yml?
- `vars_files` for general variables, `hostfile` for host variables

## To Do
- Find out more about router configuration, as well as switches

# 10 June

## Learning
- Information like `remote_user` and `ansible_become_pass` (under vars) can be included  under hostfile vars, and ansible will retrieve them automatically.
- Basic config file to work on any new switch, requires checking.

## Questions

### Q1: What to do with `command timeout`?

### Q2: Any bugs with my existing playbooks?

### Q3: Email: bitbucket https cutover??

## To Do
- Try different users and permissions on BitBucket
- Use playbooks on multiple hosts

# 12 June Summary

1. Read about layer 2 and 3 switches
2. Configured switches on EVE lab, including understanding the basic configurations needed for switch to be connectable from Ansible engine.
3. Wrote static playbooks
4. Wrote dynamic playbooks that initially just take in variable commands, later used hostfile variables as well for automatic login.
5. Connected to 2 EVE switches via playbooks
6. On chapter 8 of Juniper course
7. Have been taking notes for the course

## Future To-Dos
- pass exam and get cert
- understand how other companies do pipeline
- 50 playbooks
- final deliverable to contribute to company
- governance, workflow: document, comment, research?

# 16 June

## Learnings

### Jinja

- Imperative, give specific commands step by step
- Use three directories: tasks, templates, vars
- Used to write custom modules
    - Some devices are legacy/ modules don't exist for the device/ not ready to use the existing declarative methods.
    
## To Do
- Write a playbook with Jinja template

# 17 June

## Learnings
- Used Jinja template for existing playbook, and successfully recorded configuration of switches

## To Do
- Use template results as source for actual configuration
- Write more playbooks for configurations
- Attempt Jinja inheritance

# 24 June

## Learnings
### Command, Shell, Raw
- `command`: executes command in the *same shell* as the other tasks
- `shell`: executes command in *new shell*, for more complex commands like using pipes
- `raw`: executes *low level* commands where interpreter is missing on target host

### Playbooks
- learnt to use debug with message, finding out that `var=` requires a defined variable.

## Questions
### Q1: Why is raw used in 1.yml, does the host not have the interpreter?
Yes

### Q2: Are the extra variables in a mapping custom attributes?

## Future To-Dos
- Bring playbooks to BitBucker
- Integrate BitBucket with Ansible Tower
- Webhook to MS Teams

# 25 June
## Learnings
- send output from lab playbooks to ms teams

# 30 June
## Learnings
### RO playbook
- Automatically fetches information from critical devicees
- Entity team tracks the devices and their relevant information

### Raw
- To run multiple `raw` commands, pass them together under a single variable "{{ commands }}"

# 7 July
## Learnings
### Ansible
- `debug` automatically runs for all hosts
    - `inventory_hostname` changes to match each current host it is debugging.
        - `var=hostvars["{{ inventory_hostname }}"]` loops through all hosts and prints once from each host.
        - `var=hostvars["{{ item }}"]`
          `with_items: "{{ groups['all'] }}"` is a nested loop. Each `inventory_host` --> each `host` in `all` --> `print(hostvars[host]`.
- `add_host`: does NOT automatically loop for all hosts.
    - `inventory_hostname` == first host.
- use `serial: 1` to ensure sequential execution **host by host**.
    - when this is used, `hostvars[item]` `with_items: "{{ groups['all'] }}"` results in undefined variable for hosts other than the first one.
    - must use `hostvars[inventory_hostname]` instead

# 13 July
## Learnings
- My switch has Linux software: is an emulator
- Cisco is actually Unix based
- Wrote script for backup

## To Do
- Using `sh logs` check for bgp/ ospf/ power/ int/ module down
- Ping neighbours from host switch to confirm, regardless of logs
- Send health status to chat, excluding the actual logs

# 14 July
## Learnings
### Load Balancer
- distributes connections among servers available
- when a connection is made to an application, the connection is actually made to the load balancer, which then decides the server to connect to.

### Pool
- state: present/ absent
- snat: to protect server from internet
- lb method: round robin, cookie based, etc
- adding pool member: IP-addr based, fqdn (uses domain name, following hierarchy in DNS)

### Devices on cloud (AWS)
- management: routed through data center, using jumphost (under VPN) -> cloud
- client traffic: direct connection to lb on cloud

# 16 July
## Presentation
- raw commands only can run 1 command
- we then used bipip commands to run multiple commands
- whether sending the msg out shld be a separate task
- think from client pov, might be better to put everything under 1 task just to ensure sequential execution
- readability of playbooks only matter to developers, and to the client only the output matters
- make sure everything is easily accessible, and directory only contains what's needed for presentation
- test multiple times first!!
- explanations should not be too technical; what matters more is performance: what does it do, what's the benefit of using it
    - managers don't care about the actual code

- our purpose: show that we can run multiple commands on multiple devices -> playbook efficient!!
    - highlight that if we only write one command in one playbook, we have to write like 25 playbooks for 5 commands on 5 devices
    - easier to manage if we consolidate everything into 1 playbook
    - space efficient to use fewer playbooks as well
    
# 24 July
## Automation Pipeline
1. Users must work only on existing playbooks, and only modify the variable file to apply the playbook to the actual host.
    - One specific playbook for one specific use case.
    - User only has access to Development branch.
    - Modification reviewed by NTT Team. Each member specialises in a specific type of host (eg F5, firewall, etc)
    - SIA then has the final authority for merging Dev to Prod branch.
    - Takes about 3 days, to further reduce delay, there will be pre-approved playbooks/ use cases. Other parts of pipeline automatically triggered when users requests for relevant pull requests.
        - 3 --> 1/2 day.
    
2. Bitbucket sends webhook to Jenkins

3. Jenkins calls Ansible Tower
    - Ansible Tower fetches playbook from Bitbucket (already integrated).
    - Triggers template to start job --> pushes changes onto devices.
    
## BitBucket Integration with Ansible Tower
- Ansible Tower makes API call to the BitBucker.
    - If BitBucket account is private, `Credential` is needed for connection.
    - BitBucket link provided in `Project --> SCM URL`.
- Specify branch to pull from.
- Refresh on update to always update the template.

### Templates
- Only playbooks containing `hosts` will be visible under `Playbook`.
- `Credential` here is to connect to device and NOT the SCM.
- No direct integration of webhook between BitBucket and Ansible, so need Jenkins. Then can `enable webhook` under `Options`.