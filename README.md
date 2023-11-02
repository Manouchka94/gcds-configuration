gcds_configuration
=========

This role configure GCDS (Google Cloud Directory Sync) tools based on Jinja template of XML config file.

- **If the new config is correct** => the old config file is uploaded to Nexus Repository Server (Raw repository).
And the value of oAuth2RefreshToken and authCredentialsEncrypted is deleted from saved config file.

- **If the new config is incorrect** => the old config file is restored.

Requirements
------------

GCDS CLI should be already installed on the remote host.

Role Variables
--------------

##### defaults/main.yml

| Variable                      | Required | Default                                        |  Comments                                                             |
|-------------------------------|----------|------------------------------------------------|-----------------------------------------------------------------------|
| gcds_install_folder_path      | no       | /opt/GoogleCloudDirSync                        |  GCDS installation folder path                                        |
| gcds_config_file_name         | no       | config-gcds                                    |  GCDS Config file name                                                |
| gcds_cron_sync_script_path    | no       | /opt/GoogleCloudDirSync/gcds_synchro.sh        |  GCDS cron job script file path                                       |
| gcds_cron_job_config          | no       | see values in defaults/main.yaml               |  Cron job config for GCDS retry script                                |
| gcds_user                     | no       | root                                           |  GCDS default user to set permission for config file and retry script |


##### Extra variables from ansible-playbook CLI

All these variables should be shared using the --extra-vars ansible-playbook CLI parameter.

| Variable                      | Required | Default    |  Comments                                                             |
|-------------------------------|----------|----------- |-----------------------------------------------------------------------|
| backup_server_username        | yes      | no         |  Backup server username                                               |
| backup_server_password        | yes      | no         |  Backup server password                                               |
| backup_server_url             | no       | no         |  Remote backup server URL to backup previous GCDS config              |
| oauth2_refresh_token          | yes      | no         |  OAuth2 Token create by GCP Super Admin                               |
| auth_credentials_encrypted    | yes      | no         |  LDAP password                                                        |

Dependencies
------------

No Dependencies

Example Playbook
----------------

```yaml
- hosts: gcds
  gather_facts: yes
  roles:
    - role: gcds_configuration
  become: true
  vars:
    gcds_install_folder_path: "/opt/GoogleCloudDirSync"
    gcds_config_file_name: "config-gcds"
    gcds_cron_sync_script_path: "/opt/GoogleCloudDirSync/gcds_synchro.sh"
    gcds_cron_job_config:
      name: "gcds sync"
      minute: "05,06"
      hour: "10,14,22,3"
      day: "*"
      month: "*"
      weekday: "*"
    gcds_user: "root"
```

Example call ansible-playbook
----------------

```shell
ansible-playbook playbook.yaml  --extra-vars 'backup_server_username=*******' \
                                --extra-vars 'backup_server_password=*******' \
                                --extra-vars 'oauth2_refresh_token=********' \
                                --extra-vars 'auth_credentials_encrypted=*********' \
                                --extra-vars 'backup_server_url=**********'
```

License
-------

BSD

Author Information
------------------

EDF
