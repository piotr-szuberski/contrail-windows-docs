# List of all important VMs in Windows CI

1. VMware:
    * `ci-vc`
        * vCenter Server on Windows
        * Critical
        * __REQUIRES BACKUP__
    * `ci-vc-um`
        * vCenter Update Manager
        * __REQUIRES BACKUP__

2. Base OS templates:
    * `Windows` - Install OS (manually) + instruction (manually)
        * Preferrably: automate
    * `CentOS` - Prepared manually, no instruction
        * TODO: Instruction with requirements
        * Requirements:
            * root access SSH (password-based)
            * 2 NICs
            * TODO: Verify if there are no more requirements
    * `Ubuntu` - No template, required to provision Ubuntu-based VMs
        * TODO: Instruction/automation
        * Requirements:
            * `ciadmin` user (SSH accessible, password-based)
            * `python3`
            * 1 NIC
            * TODO: Verify if there are no more requirements

3. CI templates:
    * `builder` - Ansible, Jenkins job
        * Requires Windows template
    * `tester` - Ansible, Jenkins job
        * Requires Windows template 
    * `testbed` - Ansible
        * Requires Windows template

4. Infra:
    * `builders` - Ansible, Jenkins job
    * `testers` - Ansible, Jenkins job
    * `mgmt-dhcp` - Created manually, DHCP server + configuration (172.17.0.0/24)
        * impact if down:
            * will cause failure in all production CI jobs due to lack of dhcp for testbed machines
            * will not affect demo-env, dev-env and other machines in public network
        * fix cost:
            * Automate deployment
            * Before automation, backup
            * VMware HA
    * `winci-drive` - Created manually, network drive with (probably) temporary content
        * impact if down: 
            * no impact on production CI
            * degrades debuggability of CI (cannot upload or use ready artifacts)
            * impacts ability to create builder templates
        * fix cost:
            * will need to recreate directory structure and copy contents from one of the builders
            * this process is not documented to this degree of detail (no list of dependencies)
            * Before automation, backup
            * VMware HA
    * `winci-graphite` - Created manually
        * impact if down:
            * no impact on production CI
            * degrades CI monitoring
        * fix cost:
            * Automate VM provisioning
            * Automate configuration and dashboard deployment
            * Backup needed to preserve historical data
    * `winci-monitoring` - Created manually
        * impact if down:
            * TODO: does this affect production job (namely - post stage will fail, right?)
        * fix cost:
            * TODO
    * `winci-jenkins` - Created manually, plugins + configuration
        * impact if down:
            * ABSOLUTELY CRITICAL
        * fix cost:
            * BACKUP
                * job configuration
                * job logs
                * logs server ssh keys
                * jenkins plugin list (initial version on contrail-windows-ci; need to check if works)
                * credentials
            * Automate configuration
            * VMware HA
    * `winci-mgmt` - Created manually, Python requirements.txt have to be installed
        * impact if down:
            * ABSOLUTELY CRITICAL (cannot create testenvs)
        * fix cost:
            * Automate provisioning 
                * Ubuntu configured for Ansible
                * Ubuntu configured for Python tests
                * Ubuntu configured for monitoring scripts
            * Backup
                * ansible-vault-key
                * logs server ssh keys
                * github deploy ssh key
            * VMware HA
    * `winci-vyos-mgmt`
        * impact if down:
            * ABSOLUTELY CRITICAL (cannot deploy contrail controllers)
        * fix cost:
            * Backup
            * Document configuration
            * VMware HA
    * `winci-zuulv2-production` - Created manually with Ansible script
        * impact if down:
            * ABSOLUTELY CRITICAL (cannot run jobs)
        * fix cost:
            * Already automated
                * Create prod and dev inventories for Zuul
            * Backup:
                * gerrit ssh keys
                * jenkins ssh keys
            * VMware HA
    * `winci-purgatory` - Probably created manually? What is installed there?
        * impact if down:
            * ABSOLUTELY CRITICAL (cannot run jobs)
        * fix cost:
            * pending PR for automation

5. Hosts:
    * esxi1-5
        * fix cost:
            * VMware HA on critical virtual machines
            * backup konfiguracji
    * SSD/SATA drivers
        * fix cost:
            * TODO: Determine if servers can into RAID
            * Maintenance to configure RAID
            * If not RAID, then shared storage for critical VMs is needed
                * Preferrably some storage array with RAID configured
    * NICs
        * we lack redundancy in case of main NIC failure

6. Backup:
    * Additional storage space for backup, independent of VMware storage
    * TODO: Backup method?
    * TODO: Backup tool?
    * TODO: Backup procedure?
    * TODO: DR procedure for critical VMs?
    * TODO: hardware monitoring?