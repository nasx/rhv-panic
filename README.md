
# Red Hat Virtualization (RHV) HA VM Recovery Tester
This playbook can be used to test recovery time of an HA enabled VM inside RHV. It works by forcing a kernel panic on a hypervisor. Once panic ensues, it will wait for the VM to become available on another hypervisor.

## Variables
Place these files in a variables file (`settings.yml` use for this example) with the appropriate values.

|Variable Name|Description|Examples|
|:---|:---|:---|
|rhv_host|Hypervisor running our test workload|hv1.rhv.local|
|rhv_havm|HA enabled virtual machine|myvm.local|
|vm_test_port|TCP port used to verify VM is back on the network|22\|3389|

We also need to configure settings for the RHV manager. Ideally these would be stored in a vault, they could also be stored in `settings.yml` with the rest of our variables.

|Variable Name|Description|Examples|
|:---|:---|:---|
|rhv_manager|Hostname for the RHV manager|rhvm.rhv.local|
|rhv_manager_username|Username for the RHV manager|admin@internal|
|rhv_manager_password|Password for user defined in rhv_manager_username|s3cr3t!|

To create a vault, simply run the following command and write out the variables in yaml format.

`ansible-vault create vault.yml`

## Ansible Configuration
`ansible.cfg` is configured to time the execution of the playbook and respective tasks (see `callback_whitelist = profile_tasks, timer`).

## Running the Playbook
To run the playbook, execute the ansible-playbook command as follows:

`ansible-playbook --ask-vault-password -e @vault.yml -e @settings.yml panic.yml`
