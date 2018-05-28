# Update Zuul layout

Zuul is a triggering mechanism for Windows CI. It listens for events sent by Gerrit.
Some events make Zuul trigger specific Jenkins jobs. 
Zuul knows which events on which projects should trigger which jobs thanks to a config file.
This config file must be updated when:
* one wants to add a project on Gerrit
* one wants to change which jobs run on existing projects

For now, update process has 2 steps:
1. Commit updated `layout.yaml` file to specific Windows CI git repository.
2. Reload Zuul configuration.

### 1. Commit updated `layout.yaml` file to specific Windows CI git repository.

1. Clone `github.com/Juniper/contrail-windows-ci` repository.
```
git clone https://github.com/Juniper/contrail-windows-ci
```

2. Run `update-zuul-layout.py` script from `ansible` directory.
```
cd ansible
python .\update-zuul-layout.py
```

3. Verify what changed in the `ansible/roles/zuul/files/layout.yaml` file:
```
git diff
# verify what changed
```

4. (Optional) Make any "manual" changes to `ansible/roles/zuul/files/layout.yaml` file. Do not modify automatically generated lines.
5. Commit the changes.
6. Open a PR to `development` branch of `github.com/Juniper/contrail-windows-ci`.
7. Wait for CI checks to pass.
8. Add one of Windows team members as reviewers.

### 2. Reload Zuul configuration.

1. SSH to the machine that runs the zuul daemon.
```
localhost $ ssh winci-mgmt
```

2. Checkout newest version of `development` branch of `github.com/Juniper/contrail-windows-ci`.
```
winci-mgmt $ cd ~/ji/juniper-contrail-windows-ci
winci-mgmt $ git checkout development
winci-mgmt $ git pull
```

3. Verify that commit with updated `layout.yaml` is present.
```
winci-mgmt $ git log
```

4. Run an ansible playbook that will update and reload Zuul daemon.
```
winci-mgmt $ cd ansible
winci-mgmt $ ansible-playbook -i ~/ji/ansible.hosts play.yml --vault-password-file ~/.ansible-vault
```
