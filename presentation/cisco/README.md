# Script Structure
## Play 1
_Fetches information from switches and stores in variables._

### Preamble
- `hosts`: switches play will be run on hosts under the group switches, as stated in the host file hosts in this directory.
- `serial: 1` play will run on 1 switch at a time.

### Tasks
- `ios_command` module to run commands on remote devices running Cisco IOS.
- `commands` commands to supply to the switches, same syntax as running on switch command line.
- `register` saves output of command to variable supplied, in this case being sys_output. This variable, along with its content, will be accessible within the same play.
- `debug: var=` prints the variable supplied to screen when play is running.
- `add_host` constructs a host (based on current running host) to store corresponding information fetched, allowing it to be accessible in other plays in the playbook.
    - `name: "{{ inventory_hostname }}"` name of the created host is the IP address of the current running host
        - `p_cpu_history: " ------------ MANDY {{ inventory_hostname }} Processes CPU History: ------------ \n \n {{ hostvars[inventory_hostname].sys_output.stdout_lines[0] | join('\n') }}"`
        
            first part of the information retrieved, stored with header to differentiate it from other parts. Actual content follows after two newlines.
        
        - `sys_output` is already stored under both current running host, accessible with `hostvars[inventory_hostname].sys_output`.

