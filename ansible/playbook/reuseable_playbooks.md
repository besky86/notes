想要复用文件和组织方式，在Ansible中有三种方式可以实现：includes, imports, and roles。

# 动态和静态

> If you use any import* Task (import_playbook, import_tasks, etc.), it will be static. If you use any include* Task (include_tasks, include_role, etc.), it will be dynamic.


静态权衡：http://docs.ansible.com/ansible/latest/playbooks_reuse.html

# including and importing

* All import* statements are pre-processed at the time playbooks are parsed.
* All include* statements are processed as they encountered during the execution of the playbook.

# Importing Playbooks

```
---
- import_playbook: webservers.yml
- import_playbook: databases.yml
```

# Including and Importing Task Files

```
tasks:
- import_tasks: common_tasks.yml
# or
- include_tasks: common_tasks.yml
```

设置变量：
```
tasks:
- import_tasks: wordpress.yml wp_user=timmy
- import_tasks: wordpress.yml wp_user=alice
- import_tasks: wordpress.yml wp_user=bob
```
或是
```
tasks:
- include_tasks: wordpress.yml
  vars:
    wp_user: timmy
    ssh_keys:
    - "{{ lookup('file', 'keys/one.pub') }}"
    - "{{ lookup('file', 'keys/two.pub') }}"
```
