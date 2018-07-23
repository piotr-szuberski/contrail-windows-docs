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
    * for Windows nodes use `bms_win` dict instead of regular `bms`
    * roles supported on Windows are `vrouter` and `win_docker_driver`
    * `WINDOWS_PHYSICAL_INTERFACE` should be set to dataplane interface alias
    * specify `WINDOWS_ENABLE_TEST_SIGNING` if you want to install untrusted artifacts
    * `WINDOWS_DEBUG_DLLS_PATH` should be set to path on Ansible machine containing MSVC 2015 debug dlls, specifically: `msvcp140d.dll`, `ucrtbased.dll` and `vcruntime140d.dll`
    * refer to `config/instances.yaml.bms_win_example` for an example
  * Proceed with running Ansible playbooks:
    * `sudo -H ansible-playbook -i inventory/ playbooks/configure_instances.yml`
    * `sudo -H ansible-playbook -i inventory/ playbooks/install_contrail.yml`