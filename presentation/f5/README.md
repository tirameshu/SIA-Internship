# Script Structure
## Play 1
*Fetches information from F5 and stores in variables.*

### Preamble
- `hosts: f5` play will be run on hosts under the group `f5`, as stated in the host file hosts in this directory.

### Tasks

#### Task 1
- [`bigip_command`](https://docs.ansible.com/ansible/latest/modules/bigip_command_module.html#bigip-command-module) module to run TMSH or BASH command to a BIG-IP node and returns the results read from the device.
    - `server: "{{ inventory_hostname }}"` commands run on current host.
    - `validate_certs: no` there is no SSL certificate on Ansible Engine, cannot verify certificate. F5 will temrinate connection if this is set to yes.
        - Authentication is done only with username and password as supplied in hostfile.
- `commands` commands to supply to the F5 device
- `register` saves output of all commands in the 1 variable supplied, in this case being `sys_output`. This variable, along with its contents, will be accessible within the same play.

#### Task 2
- `debug: var=` prints the variable supplied to screen when play is running.

#### Task 3
- `add_host` constructs a host to store corresponding information fetched, allowing it to be accessible in other plays in the playbook. As there is only 1 device in this case, a single host `DUMMY HOST` is created.
    - `sys_hardware: "{{ sys_output.stdout[0] }}"` output of first command
        - `stdout` is a nested list. The first layer is a list of lists, where 1 list is the output of 1 command. The second layer is a list containing the output, already joined with `\n`.

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

#### Task 1
*`sys_hardware`*

- [`cisco_sparks`](https://docs.ansible.com/ansible/latest/modules/cisco_spark_module.html) module to connect to Cisco Webex Teams
    - variables are to be in the format `"{{ var_name }}"`
- `message: " ------------ System Hardware: ------------ \n {{ hostvars['DUMMY_HOST']['sys_hardware'] }}"`
    retrieves `sys_hardware` output stored under the `hostvars` of the dummy host.
    
Tasks repeat in a similar fashion for the other outputs.