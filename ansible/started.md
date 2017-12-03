# 1. 远程连接说明
Ansible默认远程连接为ssh,通常我们使用 ssh 与托管节点通信，默认使用 sftp.如果 sftp 不可用，可在 ansible.cfg 配置文件中配置成 scp 的方式. 


# 2. 管理节点前提：
编辑(或创建)/etc/ansible/hosts 并在其中加入一个或多个远程系统.你主机的public SSH key必须在这些系统的``authorized_keys``中:
```
192.168.1.50
aserver.example.org
bserver.example.org
```

# 3. 第一个命令：
ping 配置文件中的所有节点
 ansible all -m ping [-u username]

 ```ansible all -m ping -u bruce --sudo --sudo-user batman```
 ```ansible all -m ping -u bruce -b --become-user batman```:

