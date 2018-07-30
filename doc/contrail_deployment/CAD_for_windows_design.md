# Contrail-ansible-deployer for Windows design document
This is a short document describing initial support of deploying Windows compute nodes with contrail-ansible-deployer.
## Supported orchestrators:
  * `Openstack` - keystone is needed by Windows Docker driver for authorization

## Playbooks:
  * `provision_instances.yml` - not supported at this moment
  * `configure_instances.yml`
  * `install_openstack.yml`
  * `install_contrail.yml`

## OS-specific roles:
  * There are particular roles which needs to be done differently for Windows
  * Thus, these roles are divided into `*_Linux` and `*_Win32NT` for usage in c-a-d for Linux and c-a-d for Windows respectively

## Configure_instances:
  * Some differences between Windows and non-Windows c-a-d workflows exist for this playbook:
    * Windows compute nodes need Windows-specific dependencies and particular features turned on/off. `Install_software_Win32.yml` is responsible for that
    * In the future, vRouter kernel module will be digitally signed, but for now, it uses self-signed certificates.
    To do that, Windows Test-Signing Mode must be enabled and it can only be done after reboot

## Install_openstack:
  * There aren't any changes to this playbook
  * Whole openstack is installed for now, but in the future there should be need to specify only the role in `config/instances.yaml` which is responsible for installing keystone

## Install_contrail:
  * For debugging, specific dlls need to be present in main Windows system directory (`C:/Windows/System32`)
  * Because on Windows only Contrail compute nodes are supported, there are 2 supported roles:
    * `vrouter` - installs vRouter kernel module, vRouter agent and utils (`create_vrouter_Win32NT.yml`)
      * Pulls artifacts from `contrail-windows-vrouter` image. Artifacts' directory structure:
        * `C:/Artifacts/`
          * `agent/`
            * `contrail-vrouter-agent.msi` - installer for vRouter agent
          * `vrouter/`
            * `utils.msi` - installer for vRouter utils
            * `vRouter.msi` - installer for vRouter kernel module
            * `vRouter.cer` - certificate for vRouter kernel module
          * `vtest/*` - vtest utility (needed in Windows CI)
    * `win_docker_driver` - installs Windows Docker driver (`create_win_docker_driver.yml`)
      * Pulls artifact from `contrail-windows-docker-driver` image. Artifact's directory structure:
        * `C:/Artifacts/`
          * `docker_driver/docker-driver.msi` - installer for Windows Docker driver
  * On Windows contrail-ansible-deployer starts components as Windows services,
    in contrast to Linux where they reside in separate containers, because:
    * No possibility to run Linux containers on Windows Server 2016
    * On other Windows versions, containerization of Windows Compute components wasn't tested and implemented yet
    * There are plans to do it in future release
    * Note: potential challenges:
      * Security -  No priviliged containers support in Docker for Windows would result in all containers having access to the shared memory which is implementation of vRouter *flow0* interface
      * Communication - at this moment there is no knowledge about Docker for Windows supporting container communication through Windows named pipe which is implementation of vRouter *pkt0* and *ksync* interface