# How to deploy Contrail with Windows compute nodes

## Prerequisites

* Machine with CentOS 7.4 installed
* Machine(s) with Windows Server 2016 and updates installed
* Machine for running Ansible playbooks (Linux or Windows with WSL)

## Steps

* On the Ansible machine:
  * `git clone git@github.com:Juniper/contrail-ansible-deployer.git`
  * `cd contrail-ansible-deployer`
  * `vim config/instances.yaml`
    * To deploy a windows compute node:
      * Use `bms_win` dict instead of regular `bms`
      * Roles supported on Windows are `vrouter` and `win_docker_driver`. Specify them.
      * Set `WINDOWS_PHYSICAL_INTERFACE` to dataplane interface alias
      * Specify `WINDOWS_ENABLE_TEST_SIGNING` if you want to install untrusted artifacts (this is needed at this moment)
      * Set `WINDOWS_DEBUG_DLLS_PATH` to path on Ansible machine containing MSVC 2015 debug dlls, specifically: `msvcp140d.dll`, `ucrtbased.dll` and `vcruntime140d.dll`
    * To deploy a controller for windows compute nodes:
      * Configure as it would be a controller node for a linux ecosystem.
      * For now docker driver on windows needs keystone set up on controller:
        * Add openstack-* roles to the controller node and set `CLOUD_ORCHESTRATOR` to `openstack`.
        * Fill keystone credentials and kolla config. Check `config/instances.yaml.openstack_example`
    * Refer to `config/instances.yaml.bms_win_example` or `config/instances.yaml.bms_win_full_example` for an example
  * Proceed with running Ansible playbooks:
    * `sudo -H ansible-playbook -e orchestrator=openstack -i inventory/ playbooks/configure_instances.yml`
    * `sudo -H ansible-playbook -e orchestrator=openstack -i inventory/ playbooks/install_contrail.yml`