执行playbook:
```
ansible-playbook playbook.yml -f 10
```

# Example


```
---
- hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  remote_user: root
  tasks:
  - name: ensure apache is at the latest version
    yum:
      name: httpd
      state: latest
  - name: write the apache config file
    template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf
    notify:
    - restart apache
  - name: ensure apache is running
    service:
      name: httpd
      state: started
  handlers:
    - name: restart apache
      service:
        name: httpd
        state: restarted
```
一个Playbooks可以包含多个plays, plays 自上而下执行


```
---
- hosts: webservers
  remote_user: root
```
* hosts: Patterns, 指定group, server
* remote_user: 远程用户名, 也可以在每个task中：
```
---
- hosts: webservers
  remote_user: root
  tasks:
    - name: test connection
      ping:
      remote_user: yourname
```

# Task List

每个play包含一个tasks列表, 每次只执行一个tasks, 需要一个tasks在所有主机上执行完成后，才进行下一个tasks.

模块的执行具有幂等性，如何验证幂等性：
> One way to achieve idempotency is to have a module check whether its desired final state has already been achieved, and if that state has been achieved, to exit without performing any actions. If all the modules a playbook uses are idempotent, then the playbook itself is likely to be idempotent, so re-running the playbook should be safe.


几种执行方式：
1. action: module options
```
tasks:
  - name: make sure apache is running
    service: name=httpd state=started
```
2. command and shell modules:
```
tasks:
  - name: enable selinux
    command: /sbin/setenforce 1
```
如果这种需要忽略错误：
```
tasks:
  - name: run this command and ignore the result
    shell: /usr/bin/somecommand
    ignore_errors: True
```


## 使用变量：
```
tasks:
  - name: create a virtual host file for {{ vhost }}
    template: src=somefile.j2 dest=/etc/httpd/conf.d/{{ vhost }}
```



# Handlers：当有改变时处理

Ansible会识别主机上的变动，notify模块将调用Handlers，
```
- name: template configuration file
  template: src=template.j2 dest=/etc/foo.conf
  notify:
     - restart memcached
     - restart apache
```
```
handlers:
    - name: restart memcached
      service: name=memcached state=restarted
    - name: restart apache
      service: name=apache state=restarted
```
