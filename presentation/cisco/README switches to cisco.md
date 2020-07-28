# Script Structure
## Play 1
*Fetches information from switches and stores in variables.*

### Preamble
- `hosts: switches` play will be run on hosts under the group `switches`, as stated in the host file hosts in this directory.
- `serial: 1` play will run on 1 switch at a time.

### Tasks
#### Task 1

- `ios_command` module to run commands on remote devices running Cisco IOS.
- `commands` commands to supply to the switches, same syntax as running on switch command line.
- `register` saves output of all commands in the 1 variable supplied, in this case being `sys_output`. This variable, along with its contents, will be accessible within the same play.

#### Task 2
- `debug: var=` prints the variable supplied to screen when play is running.

#### Task 3
- `add_host` constructs a host (based on current running host) to store corresponding information fetched, allowing it to be accessible in other plays in the playbook.
    - `name: "{{ inventory_hostname }}"` name of the created host is the IP address of the current running host
        - `p_cpu_history: " ------------ MANDY {{ inventory_hostname }} Processes CPU History: ------------ \n \n {{ hostvars[inventory_hostname].sys_output.stdout_lines[0] | join('\n') }}"`
        
            first part of the information retrieved, stored with header to differentiate it from other parts. Actual content follows after two newlines.
        
            -  Actual content in `sys_output`, which is already stored under current running host, accessible with `hostvars[inventory_hostname].sys_output`.
            - `stdout_lines` is a nested list. The first layer is a list of lists, where 1 list is the output of 1 command. The second layer is a list of strings, where each string is one line of the output. These strings can be joined with `\n` so that each string appears on a new line.

After the above tasks are completed for switch 1, they are repeated for switch 2.

## Play 2
*Sends information to Cisco Webex Teams*

### Preamble
- `hosts` changed to localhost now, as the information is sent out from localhost and not the switches.
- `vars_files` file containing variables that will be accessed in this play.
    - `recipient_id`
    - `personal token`
- `credentials.yml` file in current directory containing credentials to connect to the desired Cisco Webex Teams group.

### Tasks
- [`cisco_sparks`](https://docs.ansible.com/ansible/latest/modules/cisco_spark_module.html) module to connect to Cisco Webex Teams
    - variables are to be in the format `"{{ var_name }}"`
- `with_items: "{{ groups['switches'] }}"` loops through *all hosts* in group `switches` in hostfile.
- `message: " {{ hostvars[item]['p_cpu_history'] }} , {{ hostvars[item]['ip_int_br'] }}, {{ hostvars[item]['int_status'] }} | join('\n \n \n') "`
    retrieves the three parts of output stored under current host in loop, joins them with newlines.