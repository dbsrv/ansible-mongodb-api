# ansible-mongodb-api
Install MongoDB thru Ops Manager API

###example hosts file
```bash
[mongo-master]
phelmondbbi002.karmalab.net

[mongo-replicas]
phelmondbbi003.karmalab.net
phelmondbbi004.karmalab.net

[mongo:children]
mongo-master
mongo-replicas
```

###example play
```bash
- name: mongodb
  hosts: mongo
  become: yes
  gather_facts: yes
  roles:
    - ansible-mongodb-api
  vars:
      ### (Quotes are not needed for variable values) ###

      ### MongoDB variables ###
      mongodb_version: 3.2.9 # must specify minor version
      mongodb_user: mongod
      mongodb_net_port: 27600
      mongodb_storage_dbpath: /data/apitest
      mongodb_systemlog_path: /data/apitest/log/mongod.log
      mongodb_replication_replset: apitest

      ### Ops Manager variables ###
      mongodb_install_mms_agent: false
      mongodb_mms_agent_pkg: http://phelmondbbi001:8080/download/agent/automation/mongodb-mms-automation-agent-manager-latest.x86_64.rpm
      mongodb_mms_group_id: 5759e525e4b088845ebcfabd
      mongodb_mms_api_key: 288b9b0024e33ba8d46c479295667c9e
      mongodb_mms_base_url: http://phelmondbbi001:8080

      ### Ops Manager API variables ###
      mongodb_opsmgr_api_user: ansible@expedia.com
      mongodb_opsmgr_api_key: 56e59981-8a1e-48d1-8514-8cbc11dc282d

      ### Host to modify automation configuration ###
      mongodb_master_node: "{{groups['mongo-master'][0]}}"
```

###example ansible.cfg
```bash
[defaults]
roles_path =                 ../roles/internal:../roles/external
host_key_checking =          False
retry_files_enabled =        False
command_warnings =           False
```
