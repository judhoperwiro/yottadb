## Ansible Module for installation YottaDB
---
#### Prerequisite

- ansible 2.9
- vim

### 1.) Create Directory for Elasticsearch

**NOTE**
- Please copy installation file into path /data/ansible/yottadb/source/

 ```shell
mkdir -p /data/ansible/yottadb/source/
cd /data/ansible/yottadb
vi install-yottadb-1-30.yaml
vi inventory
```
### 2.) Run Ansible Playbook  
```shell
ansible-playbook -i inventory install-yottadb-1-30.yaml
```
