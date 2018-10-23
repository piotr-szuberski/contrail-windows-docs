# Merge development to production

This document describes the steps required to perform a merge from `development` to `production` in Contrail Windows CI.

## Merge development to production

1. Update production branch (following git operations require administrative privileges on GitHub)

        git fetch --all --prune
        git checkout development
        git pull
        git checkout production
        git merge development
        git push

2. Update Zuul configuration

    - Refer to [Update Windows CI Zuulv2][update-zuulv2] documentation

### Zuul update playbook

    ---
    - hosts: zuul
      remote_user: ciadmin
      become: yes
      roles:
      - zuul

## Post checks

1. Check Zuul services:
    - `zuul-merger`
    - `zuul-server`

            localhost $ ssh winci-zuulv2-production # with provided credentials; current address 10.84.12.75
            systemctl status zuul-merger.service # should be active (running)
            systemctl status zuul-server.service # should be active (running)

2. Trigger Zuul job
    - Create a dummy PR on Gerrit
    - Post `recheck windows` on some existing PR

## Future considerations

- Slave VM templates creation and promotion
- Builder/tester deployment

[update-zuulv2]: https://juniper.github.io/contrail-windows-docs/For%20CI%20admins/CI/Update_Zuul/
