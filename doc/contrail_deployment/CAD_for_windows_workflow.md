# Workflow of contrail-ansible-deployer for windows

## Supported orchestrators:
  * `Openstack` - keystone is needed by windows docker driver

## Playbooks:
  * `provision_instances.yml` - not supported at this moment
  * `configure_instances.yml`
  * `install_openstack.yml`
  * `install_contrail.yml`

## OS-specific roles:
  * There are particular roles which needs to be done differently for Windows
  * Thus we divide these roles into `*_Linux` and `*_Win32NT` for usage in CAD for linux and CAD for windows respectively.

## Configure_instances:
  * Some differences between windows and non-windows CAD workflow exist for this playbook:
    * Windows compute nodes need Windows-specific dependencies and particular features turned on/off. `Install_software_Win32.yml` is responsible for that.
    * certificats?

## Install_openstack:
  * There aren't any changes to this playbook
  * It is needed, because windows docker driver uses keystone for authorization
  * We install whole openstack for now, but in the future we want to specify only the role in `config/instances.yaml` which is responsible for installing keystone

## Install_contrail:
  * Because on windows we support only contrail compute nodes, there are only 2 supported roles:
    * `vrouter` - install vRouter extension, vRouter agent and utils (`create_vrouter_Win32NT.yml`)
    * `win_docker_driver` - installs windows docker driver (`create_win_docker_driver.yml`)
  * On Windows contrail-ansible-deployer starts components as a windows services,
    in contrast to linux where they reside in separate containers
  * For now user of CAD needs to have docker repository with windows image and artifacts inside the image to deploy windows compute node