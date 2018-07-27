# Workflow of contrail-ansible-deployer for Windows

## Supported orchestrators:
  * `Openstack` - keystone is needed by Windows docker driver

## Playbooks:
  * `provision_instances.yml` - not supported at this moment
  * `configure_instances.yml`
  * `install_openstack.yml`
  * `install_contrail.yml`

## OS-specific roles:
  * There are particular roles which needs to be done differently for Windows
  * Thus we divide these roles into `*_Linux` and `*_Win32NT` for usage in c-a-d for Linux and c-a-d for Windows respectively.

## Configure_instances:
  * Some differences between Windows and non-Windows c-a-d workflow exist for this playbook:
    * Windows compute nodes need Windows-specific dependencies and particular features turned on/off. `Install_software_Win32.yml` is responsible for that.
    * For now, we use self-signed certificats. To do that, Windows Test-Signing Mode must be enabled and it can only be done after reboot.

## Install_openstack:
  * There aren't any changes to this playbook
  * It is needed, because Windows docker driver uses keystone for authorization
  * We install whole openstack for now, but in the future we want to specify only the role in `config/instances.yaml` which is responsible for installing keystone

## Install_contrail:
  * For debugging, we need specific dlls present in main Windows system directory (`C:/Windows/System32`).
  * Because on Windows we support only contrail compute nodes, there are only 2 supported roles:
    * `vrouter` - installs vRouter extension, vRouter agent and utils (`create_vrouter_Win32NT.yml`)
      * Pulls artifacts from `contrail-windows-vrouter` image. Artifacts' file structure:
        * `C:/Artifacts/`
          * `agent/`
            * `contrail-vrouter-agent.msi`
          * `vrouter/`
            * `utils.msi`
            * `vRouter.msi`
            * `vRouter.cer`
          * `vtest/*` - vtest utility.
    * `win_docker_driver` - installs Windows docker driver (`create_win_docker_driver.yml`)
      * Pulls artifacts from `contrail-windows-docker-driver` image. Artifacts' file structure:
        * `C:/Artifacts/`
          * `docker_driver/docker-driver.msi`
  * On Windows contrail-ansible-deployer starts components as Windows services,
    in contrast to Linux where they reside in separate containers, because:
    * No possibility to run Linux containers on Windows Server 2016
    * Integration of Docker and vRouter interfaces implementations:
      * Implementation looks as follows:
        * *ksync* and *pkt0* are implemented as **named pipes** (Windows equivalent to Linux **pipe**)
        * *flow0* is implemented as **shared memory**
      * Docker for Windows:
        * No priviliged containers support
        * It is relatively new
      * Because of that running components as containers in Windows would result in:
        * **_Security issue_** - **_ALL_** containers would have access to the **shared memory**
        * **_Probably no container communication_** - at this moment we don't know if Docker for Windows supports container communication through named pipes
      * However, we want to deploy contrail components in separate containers in the future